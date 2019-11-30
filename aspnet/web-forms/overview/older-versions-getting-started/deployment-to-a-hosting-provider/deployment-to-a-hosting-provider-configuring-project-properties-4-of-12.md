---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-configuring-project-properties-4-of-12
title: 'Implantando um aplicativo Web ASP.NET com SQL Server Compact usando o Visual Studio ou o Visual Web Developer: Configurando as propriedades do projeto-4 de 12 | Microsoft Docs'
author: tdykstra
description: Esta série de tutoriais mostra como implantar (publicar) um projeto de aplicativo Web ASP.NET que inclui um banco de dados SQL Server Compact usando o Visual Stu...
ms.author: riande
ms.date: 11/17/2011
ms.assetid: 8b013630-842c-4d44-a6fc-c6be43e7210f
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-configuring-project-properties-4-of-12
msc.type: authoredcontent
ms.openlocfilehash: 6e63e75dca3d776fb9a1bd7e420ef48891daac69
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74569808"
---
# <a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-configuring-project-properties---4-of-12"></a>Implantando um aplicativo Web ASP.NET com SQL Server Compact usando o Visual Studio ou o Visual Web Developer: Configurando as propriedades do projeto-4 de 12

por [Tom Dykstra](https://github.com/tdykstra)

[Baixar o projeto inicial](https://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> Esta série de tutoriais mostra como implantar (publicar) um projeto de aplicativo Web ASP.NET que inclui um banco de dados SQL Server Compact usando o Visual Studio 2012 RC ou o Visual Studio Express 2012 RC para Web. Você também pode usar o Visual Studio 2010 se instalar a atualização de publicação na Web. Para obter uma introdução à série, consulte [o primeiro tutorial da série](deployment-to-a-hosting-provider-introduction-1-of-12.md).
> 
> Para ver um tutorial que mostra os recursos de implantação introduzidos após a versão RC do Visual Studio 2012, mostra como implantar SQL Server edições diferentes de SQL Server Compact e mostra como implantar o Azure App aplicativos Web do serviço, consulte [implantação da Web do ASP.NET usando o Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).

## <a name="overview"></a>{1&gt;Visão Geral&lt;1}

Algumas opções de implantação são configuradas nas propriedades do projeto que são armazenadas no arquivo de projeto (o arquivo *. csproj* ou *. vbproj* ). Na maioria dos casos, os valores padrão dessas configurações são o que você deseja, mas você pode usar a interface do usuário de **Propriedades do projeto** interna no Visual Studio para trabalhar com essas configurações se precisar alterá-las. Neste tutorial, você revisará as configurações de implantação em **Propriedades do projeto**. Você também cria um arquivo de espaço reservado que faz com que uma pasta vazia seja implantada.

## <a name="configuring-deployment-settings-in-the-project-properties-window"></a>Definindo as configurações de implantação na janela Propriedades do projeto

A maioria das configurações que afetam o que acontece durante a implantação são incluídas no perfil de publicação, como você verá nos tutoriais a seguir. Algumas configurações das quais você deve estar atento estão localizadas nas guias **empacotar/publicar** da janela **Propriedades do projeto** . Essas configurações são especificadas para cada configuração de compilação — ou seja, você pode ter configurações diferentes para uma compilação de versão do que você tem para uma compilação de depuração.

Em **Gerenciador de soluções**, clique com o botão direito do mouse no projeto **ContosoUniversity** , selecione **Propriedades**e, em seguida, selecione a guia **empacotar/publicar Web** .

![Package_Publish_Web_tab](deployment-to-a-hosting-provider-configuring-project-properties-4-of-12/_static/image1.png)

Quando a janela é exibida, o padrão é mostrar as configurações para qualquer configuração de compilação atualmente ativa para a solução. Se a caixa de **configuração** não indicar **ativo (versão)** , selecione **liberar** para exibir as configurações para a configuração de Build de versão. Você implantará compilações de versão em seus ambientes de teste e produção.

![Package_Publish_Web_tab_selecting_Release](deployment-to-a-hosting-provider-configuring-project-properties-4-of-12/_static/image2.png)

Com **Active (versão)** ou **versão** selecionada, você vê os valores que são efetivos ao implantar usando a configuração de Build de versão:

- Na caixa **itens a serem implantados** , **somente os arquivos necessários para executar o aplicativo** são selecionados. Outras opções são **todos os arquivos neste projeto** ou **todos os arquivos nesta pasta do projeto**. Deixando a seleção padrão inalterada, você evita a implantação de arquivos de código-fonte, por exemplo. Essa configuração é o motivo pelo qual as pastas que contêm os arquivos binários SQL Server Compact tinham que ser incluídas no projeto. Para obter mais informações sobre essa configuração, consulte **por que nem todos os arquivos na pasta do meu projeto são implantados?** em [perguntas frequentes sobre implantação do projeto de aplicativo Web ASP.net](https://msdn.microsoft.com/library/ee942158.aspx).
- **Excluir símbolos de depuração gerados** está selecionado. Você não estará Depurando quando usar essa configuração de compilação.
- **Excluir arquivos da pasta de dados do\_de aplicativos** não está selecionado. O arquivo de SQL Server Compact para o banco de dados de associação está nessa pasta e você precisa implantá-lo. Ao implantar atualizações que não incluem alterações de banco de dados, você marcará essa caixa de seleção.
- **Pré-compile este aplicativo antes de publicar** não está selecionado. Na maioria dos cenários, não há necessidade de pré-compilar projetos de aplicativos Web. Para obter mais informações sobre essa opção, consulte a [guia pacote/publicar Web, propriedades do projeto](https://msdn.microsoft.com/library/dd410108(v=vs.110).aspx) e [Configurações avançadas de pré-compilação](https://msdn.microsoft.com/library/hh475319(v=vs.110).aspx).
- **Incluir todos os bancos de dados configurados na guia pacote/publicar SQL** está selecionado, mas essa opção não tem efeito agora porque você não está configurando a guia **pacote/publicar SQL** . Essa guia destina-se a um método de implantação de banco de dados herdado que costumava ser a única opção para a implantação de dados de SQL Server. Você usará a guia **empacotar/publicar SQL** no tutorial [migrando para SQL Server](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12.md) .
- A seção **configurações do pacote de implantação da Web** não se aplica porque você está usando a publicação com um clique nesses tutoriais.

Altere a caixa suspensa **configuração** para depurar a fim de ver as configurações padrão para compilações de depuração. Os valores são os mesmos, exceto que **excluir símbolos de depuração gerados** é limpo para que você possa depurar ao implantar uma compilação de depuração.

## <a name="making-sure-that-the-elmah-folder-gets-deployed"></a>Certificando-se de que a pasta do ELMAH é implantada

Como vimos no tutorial anterior, o [pacote NuGet do ELMAH](http://www.hanselman.com/blog/NuGetPackageOfTheWeek7ELMAHErrorLoggingModulesAndHandlersWithSQLServerCompact.aspx) fornece funcionalidade para registro em log de erros e relatórios. No aplicativo da Contoso University, o ELMAH foi configurado para armazenar detalhes do erro em uma pasta chamada *ELMAH*:

![Pasta do ELMAH](deployment-to-a-hosting-provider-configuring-project-properties-4-of-12/_static/image3.png)

Excluir arquivos ou pastas específicas da implantação é um requisito comum; outro exemplo seria uma pasta na qual os usuários podem carregar arquivos. Você não quer arquivos de log ou arquivos carregados que foram criados em seu ambiente de desenvolvimento para serem implantados na produção. E se estiver implantando uma atualização para produção, você não deseja que o processo de implantação exclua arquivos que existem na produção. (Dependendo de como você define uma opção de implantação, se um arquivo existir no site de destino, mas não no site de origem quando você implantar o, Implantação da Web o excluirá do destino.)

Como vimos anteriormente neste tutorial, a opção **itens a serem implantados** na guia **empacotar/publicar Web** é definida **somente para os arquivos necessários para executar este aplicativo**. Como resultado, os arquivos de log criados pelo ELMAH no desenvolvimento não serão implantados, o que é o que você deseja que aconteça. (Para ser implantado, eles teriam que ser incluídos no projeto e sua propriedade de **ação de compilação** teria que ser definida como **Content**. Para obter mais informações, consulte **por que nem todos os arquivos na pasta do meu projeto são implantados?** em [perguntas frequentes sobre implantação do projeto de aplicativo Web ASP.net](https://msdn.microsoft.com/library/ee942158.aspx)). No entanto, Implantação da Web não criará uma pasta no site de destino, a menos que haja pelo menos um arquivo para copiar para ele. Portanto, você adicionará um arquivo *. txt* à pasta para atuar como um espaço reservado para que a pasta seja copiada.

Em **Gerenciador de soluções**, clique com o botão direito do mouse na pasta *ELMAH* , selecione **Adicionar novo item**e crie um arquivo chamado *PlaceHolder. txt*. Coloque o seguinte texto: "Este é um arquivo de espaço reservado para garantir que a pasta seja implantada". e salve o arquivo. Isso é tudo o que você precisa fazer para ter certeza de que o Visual Studio implanta esse arquivo e a pasta em que ele está, porque a propriedade de **ação de compilação** de arquivos *. txt* está definida como **conteúdo** por padrão.

Agora você concluiu todas as tarefas de configuração de implantação. No próximo tutorial, você implantará o site da Contoso University no ambiente de teste e o testará.

> [!div class="step-by-step"]
> [Anterior](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12.md)
> [Próximo](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12.md)
