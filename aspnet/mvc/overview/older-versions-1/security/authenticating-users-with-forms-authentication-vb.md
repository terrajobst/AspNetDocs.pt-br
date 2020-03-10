---
uid: mvc/overview/older-versions-1/security/authenticating-users-with-forms-authentication-vb
title: Autenticando usuários com a autenticação de formulários (VB) | Microsoft Docs
author: microsoft
description: Saiba como usar o atributo [autorizar] para proteger por senha páginas específicas em seu aplicativo MVC. Você aprenderá a usar a administração de site também...
ms.author: riande
ms.date: 01/27/2009
ms.assetid: 4341f5b1-6fe5-44c5-8b8a-18fa84f80177
msc.legacyurl: /mvc/overview/older-versions-1/security/authenticating-users-with-forms-authentication-vb
msc.type: authoredcontent
ms.openlocfilehash: a2c2140631d59a7f8b21aa73613a92ea5c7a91d0
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78615573"
---
# <a name="authenticating-users-with-forms-authentication-vb"></a>Autenticar usuários com a autenticação de formulários (VB)

pela [Microsoft](https://github.com/microsoft)

> Saiba como usar o atributo [autorizar] para proteger por senha páginas específicas em seu aplicativo MVC. Você aprende a usar a ferramenta de administração de site para criar e gerenciar usuários e funções. Você também aprenderá a configurar onde a conta de usuário e as informações de função são armazenadas.

O objetivo deste tutorial é explicar como você pode usar a autenticação de formulários para proteger com senha as exibições em seus aplicativos MVC do ASP.NET. Você aprende a usar a ferramenta de administração de site para criar usuários e funções. Você também aprende como impedir que usuários não autorizados invoquem ações do controlador. Por fim, você aprende a configurar onde os nomes de usuário e as senhas são armazenados.

#### <a name="using-the-web-site-administration-tool"></a>Usando a ferramenta de administração de site

Antes de fazermos qualquer outra coisa, devemos começar criando alguns usuários e funções. A maneira mais fácil de criar novos usuários e funções é aproveitar a ferramenta de administração de site do Visual Studio 2008. Você pode iniciar essa ferramenta selecionando a opção de menu **Project, ASP.NET Configuration**. Como alternativa, você pode iniciar a ferramenta de administração de site clicando no ícone (um pouco assustador) do martelo, atingindo o mundo que aparece na parte superior da janela de Gerenciador de Soluções (consulte a Figura 1).

**Figura 1 – Iniciando a ferramenta de administração de site**

![clip_image002[4]](authenticating-users-with-forms-authentication-vb/_static/image1.jpg)

Na ferramenta de administração de site, você cria novos usuários e funções selecionando a guia segurança. Clique no link **criar usuário** para criar um novo usuário chamado Stephen (veja a Figura 2). Forneça ao usuário Stephen qualquer senha que desejar (por exemplo, *segredo*).

**Figura 2 – Criando um novo usuário**

![clip_image004[4]](authenticating-users-with-forms-authentication-vb/_static/image2.jpg)

Você cria novas funções habilitando primeiro as funções e definindo uma ou mais funções. Habilite as funções clicando no link **habilitar funções** . Em seguida, crie uma função chamada *Administradores* clicando no link **criar ou gerenciar funções** (consulte a Figura 3).

**Figura 3 – criando uma nova função**

![clip_image006[4]](authenticating-users-with-forms-authentication-vb/_static/image3.jpg)

Por fim, crie um novo usuário chamado Sally e associe o Sally à função Administradores clicando no link criar usuário e selecionando administradores ao criar Sally (consulte a Figura 4).

**Figura 4 – adicionando um usuário a uma função**

![clip_image008[4]](authenticating-users-with-forms-authentication-vb/_static/image4.jpg)

Quando tudo for dito e concluído, você deverá ter dois novos usuários chamados Stephen e Sally. Você também deve ter uma nova função denominada administradores. Sally é um membro da função Administrators e Stephen não é.

#### <a name="requiring-authorization"></a>Exigindo autorização

Você pode exigir que um usuário seja autenticado antes que o usuário invoque uma ação do controlador adicionando o atributo [autorizar] à ação. Você pode aplicar o atributo [autorizar] a uma ação de controlador individual ou pode aplicar esse atributo a uma classe de controlador inteira.

Por exemplo, o controlador na Listagem 1 expõe uma ação chamada CompanySecrets (). Como essa ação é decorada com o atributo [Authorize], essa ação não pode ser invocada a menos que um usuário seja autenticado.

**Listagem 1 – Controllers\HomeController.vb**

[!code-vb[Main](authenticating-users-with-forms-authentication-vb/samples/sample1.vb)]

Se você invocar a ação CompanySecrets () inserindo a URL/Home/CompanySecrets na barra de endereços do seu navegador e não for um usuário autenticado, você será redirecionado para a exibição de logon automaticamente (veja a Figura 5).

**Figura 5 – a exibição de logon**

![clip_image010[4]](authenticating-users-with-forms-authentication-vb/_static/image5.jpg)

Você pode usar o modo de exibição de logon para inserir seu nome de usuário e senha. Se você não for um usuário registrado, poderá clicar no link **registrar** para navegar até o modo de exibição de registro (consulte a Figura 6). Você pode usar a exibição registrar para criar uma nova conta de usuário.

**Figura 6 – a exibição de registro**

![clip_image012](authenticating-users-with-forms-authentication-vb/_static/image6.jpg)

Depois de fazer logon com êxito, você poderá ver a exibição CompanySecrets (veja a Figura 7). Por padrão, você continuará a ser conectado até que você feche a janela do navegador.

**Figura 7 – a exibição CompanySecrets**

![clip_image014](authenticating-users-with-forms-authentication-vb/_static/image7.jpg)

#### <a name="authorizing-by-user-name-or-user-role"></a>Autorizando por nome de usuário ou função de usuário

Você pode usar o atributo [autorizar] para restringir o acesso a uma ação do controlador a um determinado conjunto de usuários ou a um determinado conjunto de funções de usuário. Por exemplo, o controlador inicial modificado na Listagem 2 contém duas novas ações chamadas StephenSecrets () e AdministratorSecrets ().

**Listagem 2 – Controllers\HomeController.vb**

[!code-vb[Main](authenticating-users-with-forms-authentication-vb/samples/sample2.vb)]

Somente um usuário com o nome de usuário Stephen pode invocar a ação StephenSecrets (). Todos os outros usuários são redirecionados para a exibição de logon. A propriedade Users aceita uma lista separada por vírgulas de nomes de conta de usuário.

Somente os usuários na função administradores podem invocar a ação AdministratorSecrets (). Por exemplo, como Sally é um membro do grupo Administradores, ela pode invocar a ação AdministratorSecrets (). Todos os outros usuários são redirecionados para a exibição de logon. A Propriedade Roles aceita uma lista separada por vírgulas de nomes de função.

#### <a name="configuring-authentication"></a>Configurando a autenticação

Neste ponto, você deve estar se perguntando onde a conta de usuário e as informações de função estão sendo armazenadas. Por padrão, as informações são armazenadas em um banco de dados do (RANU) SQL Express chamado ASPNETDB. MDF, localizado na pasta do aplicativo do MVC\_Data. Esse banco de dados é gerado pela estrutura ASP.NET automaticamente quando você começa a usar a associação.

Para ver o banco de dados ASPNETDB. MDF na janela de Gerenciador de Soluções, primeiro você precisa selecionar o projeto de opção de menu, mostrar todos os arquivos.

Usar o banco de dados do SQL Express padrão é bom ao desenvolver um aplicativo. No entanto, é mais provável que você não queira usar o banco de dados ASPNETDB. MDF padrão para um aplicativo de produção. Nesse caso, você pode alterar onde as informações de conta de usuário são armazenadas, concluindo as duas etapas a seguir:

1. Adicionar os objetos de banco de dados Serviços de Aplicativos ao seu banco de dados de produção-altere a cadeia de conexão do aplicativo para apontar para o banco de dados de produção

A primeira etapa é adicionar todos os objetos de banco de dados necessários (tabelas e procedimentos armazenados) ao seu banco de dados de produção. A maneira mais fácil de adicionar esses objetos a um novo banco de dados é tirar proveito do assistente de instalação do ASP.NET SQL Server (consulte a Figura 8). Você pode iniciar essa ferramenta abrindo o prompt de comando do Visual Studio 2008 do grupo de programas Microsoft Visual Studio 2008 e executando o seguinte comando no prompt de comando:

RegSql ASPNET\_

**Figura 8 – o assistente de instalação do ASP.NET SQL Server**

![clip_image016](authenticating-users-with-forms-authentication-vb/_static/image8.jpg)

O assistente de instalação do ASP.NET SQL Server permite que você selecione um banco de dados de SQL Server em sua rede e instale todos os objetos de banco de dados exigidos pelos serviços de aplicativo do ASP.NET. Não é necessário que o servidor de banco de dados esteja localizado no computador local.

> [!NOTE]
> Se você não quiser usar o assistente de instalação do ASP.NET SQL Server, poderá encontrar scripts SQL para adicionar os objetos de banco de dados dos serviços de aplicativos na seguinte pasta:
> 
> 
> C:\Windows\Microsoft.NET\Framework\v2.0.50727

Depois de criar os objetos de banco de dados necessários, você precisa modificar a conexão de banco de dados usada pelo aplicativo MVC. Modifique a cadeia de conexão ApplicationServices no arquivo de configuração da Web (Web. config) para que ele aponte para o banco de dados de produção. Por exemplo, a conexão modificada na Listagem 3 aponta para um banco de dados chamado MyProductionDB (a cadeia de conexão do ApplicationServices original foi comentada).

**Listagem 3 – Web. config**

[!code-xml[Main](authenticating-users-with-forms-authentication-vb/samples/sample3.xml)]

#### <a name="configuring-database-permissions"></a>Configurando permissões de banco de dados

Se você usar a segurança integrada para se conectar ao banco de dados, precisará adicionar a conta de usuário do Windows correta como um logon ao seu banco de dados. A conta correta depende se você está usando o ASP.NET Development Server ou Serviços de Informações da Internet como seu servidor Web. A conta de usuário correta também depende do seu sistema operacional.

Se você estiver usando o ASP.NET Development Server (o servidor Web padrão usado pelo Visual Studio), seu aplicativo será executado dentro do contexto da sua conta de usuário do Windows. Nesse caso, você precisa adicionar sua conta de usuário do Windows como um logon de servidor de banco de dados.

Como alternativa, se você estiver usando Serviços de Informações da Internet, precisará adicionar a conta ASPNET ou a conta de serviço de rede/Autoridade NT como um logon de servidor de banco de dados. Se você estiver usando o Windows XP, adicione a conta ASPNET como um logon ao seu banco de dados. Se você estiver usando um sistema operacional mais recente, como o Windows Vista ou o Windows Server 2008, adicione a conta NT AUTHORITY/NETWORK SERVICE como o logon do banco de dados.

Você pode adicionar uma nova conta de usuário ao banco de dados usando Microsoft SQL Server Management Studio (consulte a Figura 9).

**Figura 9 – criando um novo logon de Microsoft SQL Server**

![clip_image018](authenticating-users-with-forms-authentication-vb/_static/image9.jpg)

Depois de criar o logon necessário, você precisará mapear o logon para um usuário de banco de dados com as funções de banco de dados corretas. Clique duas vezes no logon e selecione a guia mapeamento de usuário. Selecione uma ou mais funções de banco de dados de serviços de aplicativo. Por exemplo, para autenticar usuários, você precisa habilitar a associação de\_do ASPNET\_função de banco de dados BasicAccess. Para criar novos usuários, você precisa habilitar a associação de\_do ASPNET\_função de banco de dados FullAccess (consulte a Figura 10).

**Figura 10 – adicionando Serviços de Aplicativos funções de banco de dados**

![clip_image020](authenticating-users-with-forms-authentication-vb/_static/image10.jpg)

#### <a name="summary"></a>Resumo

Neste tutorial, você aprendeu a usar a autenticação de formulários ao criar um aplicativo MVC do ASP.NET. Primeiro, você aprendeu a criar novos usuários e funções aproveitando a ferramenta de administração de site. Em seguida, você aprendeu a usar o atributo [autorizar] para impedir que usuários não autorizados invoquem ações do controlador. Por fim, você aprendeu como configurar seu aplicativo MVC para armazenar informações de usuário e função em um banco de dados de produção.

> [!div class="step-by-step"]
> [Anterior](preventing-javascript-injection-attacks-cs.md)
> [Próximo](authenticating-users-with-windows-authentication-vb.md)
