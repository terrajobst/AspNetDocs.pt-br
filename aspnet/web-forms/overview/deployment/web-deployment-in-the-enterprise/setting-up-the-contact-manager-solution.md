---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/setting-up-the-contact-manager-solution
title: Configurando a solução do Gerenciador de contatos | Microsoft Docs
author: jrjlee
description: Este tópico descreve como baixar e configurar a solução do Gerenciador de contatos para ser executada localmente em uma estação de trabalho do desenvolvedor.
ms.author: riande
ms.date: 05/04/2012
ms.assetid: 200b973c-776b-4a9b-9e82-39fda6120a52
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/setting-up-the-contact-manager-solution
msc.type: authoredcontent
ms.openlocfilehash: d9774ee01cb0515d7e733b24baa661f2648bd7c4
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78629853"
---
# <a name="setting-up-the-contact-manager-solution"></a>Configuração da solução Gerenciador de Contatos

por [Jason Lee](https://github.com/jrjlee)

[Baixar PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Este tópico descreve como baixar e configurar a solução do Gerenciador de contatos para ser executada localmente em uma estação de trabalho do desenvolvedor.

## <a name="system-requirements"></a>Requisitos do Sistema

Para executar a solução Contact Manager localmente e executar as outras tarefas descritas neste tutorial, você precisará instalar este software em sua estação de trabalho do desenvolvedor:

- Visual Studio 2010 Service Pack 1, Premium ou Ultimate Edition
- Serviços de Informações da Internet (IIS) 7,5 Express
- SQL Server Express 2008 R2
- Ferramenta de implantação da Web do IIS (Implantação da Web) 2,1 ou posterior
- ASP.NET 4.0
- ASP.NET MVC 3
- .NET Framework 4
- .NET Framework 3.5 SP1

Com exceção do Visual Studio 2010, você pode baixar e instalar as versões mais recentes de todos esses produtos e componentes por meio do [Web Platform Installer](https://go.microsoft.com/?linkid=9805118).

## <a name="download-and-extract-the-solution"></a>Baixar e extrair a solução

Você pode baixar o aplicativo de exemplo do Gerenciador de contatos da Galeria de códigos do MSDN [aqui](https://code.msdn.microsoft.com/Deploying-Web-Applications-9d9093c0).

## <a name="configure-and-run-the-solution"></a>Configurar e executar a solução

Para configurar e executar a solução Contact Manager em seu computador local, você precisará executar estas etapas de alto nível:

1. Se você ainda não tiver um, crie um banco de dados de serviços de aplicativo ASP.NET local com os recursos de associação e gerenciamento de função habilitados.
2. Edite cadeias de conexão nos arquivos *Web. config* para apontar para sua instância de SQL Server Express local.
3. Execute a solução do Visual Studio 2010.

O restante desta seção fornece mais diretrizes sobre como concluir cada uma dessas tarefas.

**Para criar o banco de dados de serviços de aplicativos**

1. Abra um prompt de comando Visual Studio 2010. Para fazer isso, no menu **Iniciar** , aponte para **todos os programas**, clique **em Microsoft Visual Studio 2010**, clique em **Ferramentas do Visual Studio**e, em seguida, clique em **prompt de comando do Visual Studio (2010)** .
2. No prompt de comando, digite este comando e pressione ENTER:

    [!code-console[Main](setting-up-the-contact-manager-solution/samples/sample1.cmd)]

    1. Use a opção **– C** para especificar a cadeia de conexão para o servidor de banco de dados.
    2. Use a opção **–** a para especificar os recursos dos serviços de aplicativos que você deseja adicionar ao banco de dados. Nesse caso, **m** indica que você deseja adicionar suporte para o provedor de associação e o **r** indica que você deseja adicionar suporte para o Gerenciador de função.
    3. Use a opção **– d** para especificar um nome para o banco de dados de serviços de aplicativo. Se você omitir essa opção, o utilitário criará um banco de dados com o nome padrão **aspnetdb**.
3. Quando o banco de dados tiver sido criado com êxito, o prompt de comando mostrará uma confirmação.

    ![](setting-up-the-contact-manager-solution/_static/image1.png)

> [!NOTE]
> Para obter mais informações sobre o utilitário ASPNET\_RegSql, consulte a [ferramenta de registro do ASP.NET SQL Server (aspnet\_RegSql. exe)](https://msdn.microsoft.com/library/ms229862(v=vs.100).aspx).

A próxima etapa é certificar-se de que as cadeias de conexão na solução do Gerenciador de contatos apontem para sua instância local do SQL Server Express.

**Para atualizar as cadeias de conexão**

1. Abra a solução Contact Manager no Visual Studio 2010.
2. Na janela **Gerenciador de soluções** , expanda o projeto **ContactManager. Mvc** e clique duas vezes no nó **Web. config** .

    > [!NOTE]
    > O projeto ContactManager. Mvc inclui dois arquivos *Web. config* . Você precisa editar o arquivo de nível de projeto.

    ![](setting-up-the-contact-manager-solution/_static/image2.png)
3. No elemento **connectionStrings** , verifique se a cadeia de conexão chamada **ApplicationServices** aponta para o banco de dados de serviços de aplicativo ASP.net local.

    [!code-xml[Main](setting-up-the-contact-manager-solution/samples/sample2.xml)]
4. Na janela **Gerenciador de soluções** , expanda o projeto **ContactManager. Service** e clique duas vezes no nó **Web. config** .

    ![](setting-up-the-contact-manager-solution/_static/image3.png)
5. No elemento **connectionStrings** , na cadeia de conexão chamada **ContactManagerContext**, verifique se a propriedade **fonte de dados** está definida como sua instância local do SQL Server Express. Você não precisa alterar mais nada na cadeia de conexão.

    [!code-xml[Main](setting-up-the-contact-manager-solution/samples/sample3.xml)]
6. Salve todos os arquivos abertos.

Agora você deve estar pronto para executar a solução Contact Manager em seu computador local.

> [!NOTE]
> Se você seguir essas etapas sem primeiro criar um banco de dados de serviços de aplicativo, o ASP.NET criará o banco de dados na primeira vez que você tentar criar um usuário. No entanto, a criação manual do banco de dados proporciona muito mais controle sobre o conjunto de recursos dos serviços de aplicativos que você deseja dar suporte.

**Para executar a solução Contact Manager**

1. No Visual Studio 2010, pressione F5.
2. O Internet Explorer é iniciado e solicita a URL do aplicativo Contact Manager ASP.NET MVC 3. Por padrão, o aplicativo exibe a página **todos os contatos** .

    ![](setting-up-the-contact-manager-solution/_static/image4.png)
3. Adicione alguns contatos e verifique se o aplicativo funciona conforme o esperado.

    ![](setting-up-the-contact-manager-solution/_static/image5.png)
4. Navegue até `http://localhost:50114/Account/Register` (ajuste a URL se você estiver hospedando o aplicativo em uma porta diferente). Adicione um nome de usuário, endereço de email e senha e verifique se você consegue registrar uma conta com êxito.

    ![](setting-up-the-contact-manager-solution/_static/image6.png)
5. Navegue até `http://localhost:50114/Account/LogOn` (ajuste a URL se você estiver hospedando o aplicativo em uma porta diferente). Verifique se você é capaz de fazer logon usando a conta que você acabou de criar.

    ![](setting-up-the-contact-manager-solution/_static/image7.png)
6. Feche o Internet Explorer para interromper a depuração.

## <a name="conclusion"></a>Conclusão

Neste ponto, a solução de gerente de contato deve ser totalmente configurada para ser executada no computador local. Você pode usar a solução como referência ao trabalhar com os outros tópicos deste tutorial.

O próximo tópico, [noções básicas sobre o arquivo de projeto](understanding-the-project-file.md), explica como você pode usar os arquivos de projeto do Microsoft Build Engine (MSBuild) personalizados dentro da solução Contact Manager para controlar o processo de implantação.

> [!div class="step-by-step"]
> [Anterior](the-contact-manager-solution.md)
> [Próximo](understanding-the-project-file.md)
