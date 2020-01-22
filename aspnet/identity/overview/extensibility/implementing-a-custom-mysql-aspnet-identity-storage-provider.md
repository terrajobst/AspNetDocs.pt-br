---
uid: identity/overview/extensibility/implementing-a-custom-mysql-aspnet-identity-storage-provider
title: Implementando um provedor de armazenamento de ASP.NET Identity do MySQL personalizado-ASP.NET 4. x
author: raquelsa
description: ASP.NET Identity é um sistema extensível que permite que você crie seu próprio provedor de armazenamento e conecte-o ao seu aplicativo sem retrabalhar com o aplicado...
ms.author: riande
ms.date: 05/22/2015
ms.assetid: 248f5fe7-39ba-40ea-ab1e-71a69b0bd649
ms.custom: seoapril2019
msc.legacyurl: /identity/overview/extensibility/implementing-a-custom-mysql-aspnet-identity-storage-provider
msc.type: authoredcontent
ms.openlocfilehash: 2f0b47d45bce82c71d1864536309f9e2ffed2d63
ms.sourcegitcommit: 88fc80e3f65aebdf61ec9414810ddbc31c543f04
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/22/2020
ms.locfileid: "76519122"
---
# <a name="implementing-a-custom-mysql-aspnet-identity-storage-provider"></a>Implementar um provedor de armazenamento personalizado de MySQL da Identidade do ASP.NET

por [Raquel Soares de Almeida](https://github.com/raquelsa), [Suhas Joshi](https://github.com/suhasj), [Tom FitzMacken](https://github.com/tfitzmac)

> ASP.NET Identity é um sistema extensível que permite criar seu próprio provedor de armazenamento e conectá-lo ao seu aplicativo sem retrabalhar com o aplicativo. Este tópico descreve como criar um provedor de armazenamento do MySQL para ASP.NET Identity. Para obter uma visão geral da criação de provedores de armazenamento personalizados, consulte [visão geral dos provedores de armazenamento personalizados para ASP.net Identity](overview-of-custom-storage-providers-for-aspnet-identity.md).
> 
> Para concluir este tutorial, você deve ter Visual Studio 2013 com a atualização 2.
> 
> Este tutorial irá:
> 
> - Mostre como criar uma instância de banco de dados MySQL no Azure.
> - Mostre como usar uma ferramenta de cliente MySQL (MySQL Workbench) para criar tabelas e gerenciar seu banco de dados remoto no Azure.
> - Mostre como substituir a implementação de armazenamento de ASP.NET Identity padrão por nossa implementação personalizada em um projeto de aplicativo MVC.
> 
> Este tutorial foi escrito originalmente por Raquel Soares de Almeida e Rick Anderson ( [@RickAndMSFT](https://twitter.com/#!/RickAndMSFT) ). O projeto de exemplo foi atualizado para a identidade 2,0 por Suhas Joshi. O tópico foi atualizado para a identidade 2,0 por Tom FitzMacken.

## <a name="download-completed-project"></a>Baixar projeto concluído

No final deste tutorial, você terá um projeto de aplicativo MVC com ASP.NET Identity trabalhando com um banco de dados MySQL hospedado no Azure.

Você pode baixar o provedor de armazenamento MySQL concluído em [ASPNET. Identity. MySQL (github)](https://github.com/aspnet/samples/tree/master/samples/aspnet/Identity/AspNet.Identity.MySQL).

## <a name="the-steps-you-will-perform"></a>As etapas que serão executadas

Neste tutorial, você vai:

1. Criar um banco de dados MySQL no Azure
2. Criar as tabelas de ASP.NET Identity no MySQL
3. Criar um aplicativo MVC e configurá-lo para usar o provedor MySQL
4. Executar o aplicativo

Este tópico não aborda a arquitetura do ASP.NET Identity e as decisões que você deve tomar ao implementar um provedor de armazenamento do cliente. Para obter essas informações, consulte [visão geral dos provedores de armazenamento personalizados para ASP.net Identity](overview-of-custom-storage-providers-for-aspnet-identity.md).

## <a name="review-mysql-storage-provider-classes"></a>Examinar as classes do provedor de armazenamento do MySQL

Antes de entrar nas etapas para criar o provedor de armazenamento do MySQL, vamos examinar as classes que compõem o provedor de armazenamento. Você precisará de classes que gerenciam as operações de banco de dados e as classes que são chamadas do aplicativo para gerenciar usuários e funções.

### <a name="storage-classes"></a>Classes de armazenamento

- [IdentityUser](https://github.com/aspnet/samples/blob/master/samples/aspnet/Identity/AspNet.Identity.MySQL/IdentityUser.cs) -contém propriedades para o usuário.
- [USERSTORE](https://github.com/aspnet/samples/blob/master/samples/aspnet/Identity/AspNet.Identity.MySQL/UserStore.cs) -contém operações para adicionar, atualizar ou recuperar usuários.
- [IdentityRole](https://github.com/aspnet/samples/blob/master/samples/aspnet/Identity/AspNet.Identity.MySQL/IdentityRole.cs) -contém propriedades para funções.
- [RoleStore](https://github.com/aspnet/samples/blob/master/samples/aspnet/Identity/AspNet.Identity.MySQL/RoleStore.cs) -contém operações para adicionar, excluir, atualizar e recuperar funções.

### <a name="data-access-layer-classes"></a>Classes de camada de acesso a dados

Para este exemplo, as classes de camada de acesso a dados contêm instruções SQL para trabalhar com as tabelas; no entanto, em seu código, talvez você queira usar o ORM (mapeamento relacional de objeto), como Entity Framework ou NHibernate. Em particular, seu aplicativo pode experimentar mau desempenho sem um ORM que inclui carregamento lento e cache de objetos. Para obter mais informações, consulte [ASP.NET Identity 2,0 sem Entity Framework?](https://aspnetidentity.codeplex.com/discussions/561828)

- [MySQLDatabase](https://github.com/aspnet/samples/blob/master/samples/aspnet/Identity/AspNet.Identity.MySQL/MySQLDatabase.cs) -contém a conexão de banco de dados MySQL e os métodos para executar operações de banco de dados. USERSTORE e RoleStore são instanciados com uma instância dessa classe.
- [Roletable](https://github.com/aspnet/samples/blob/master/samples/aspnet/Identity/AspNet.Identity.MySQL/RoleTable.cs) – contém operações de banco de dados para a tabela que armazena funções.
- [Userclaimtable](https://github.com/aspnet/samples/blob/master/samples/aspnet/Identity/AspNet.Identity.MySQL/UserClaimsTable.cs) – contém operações de banco de dados para a tabela que armazena declarações de usuário.
- [UserLoginsTable](https://github.com/aspnet/samples/blob/master/samples/aspnet/Identity/AspNet.Identity.MySQL/UserLoginsTable.cs) -contém operações de banco de dados para a tabela que armazena informações de logon do usuário.
- [Userroletable](https://github.com/aspnet/samples/blob/master/samples/aspnet/Identity/AspNet.Identity.MySQL/UserRoleTable.cs) – contém operações de banco de dados para a tabela que armazena quais usuários são atribuídos a quais funções.
- [Usertable](https://github.com/aspnet/samples/blob/master/samples/aspnet/Identity/AspNet.Identity.MySQL/UserTable.cs) -contém operações de banco de dados para a tabela que armazena os usuários.

## <a name="create-a-mysql-database-instance-on-azure"></a>Criar uma instância de banco de dados MySQL no Azure

1. Faça logon no [portal do Azure](https://manage.windowsazure.com/).
2. Clique em **+ novo** na parte inferior da página e selecione **armazenar**.  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image1.png)
3. No assistente **escolher e complemento** , selecione banco de **dados MySQL ClearDB** e clique na seta avançar na parte inferior direita da caixa de diálogo.  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image2.png)
4. Mantenha o plano **gratuito** padrão e altere o **nome** para **IdentityMySQLDatabase**. Selecione a região mais próxima e clique na seta avançar.  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image3.png)
5. Clique na marca de seleção para concluir a criação do banco de dados.  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image4.png)
6. Depois que o banco de dados tiver sido criado, você pode gerenciá-lo na guia **COMPLEMENTOS** no portal de gerenciamento.   
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image5.png)
7. Você pode obter as informações de conexão do banco de dados clicando em **informações de conexão** na parte inferior da página.  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image6.png)
8. Copie a cadeia de conexão clicando no botão Copiar e salve-a para que você possa usá-la posteriormente em seu aplicativo MVC.   
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image7.png)

## <a name="create-the-aspnet-identity-tables-in-a-mysql-database"></a>Criar as tabelas de ASP.NET Identity em um banco de dados MySQL

### <a name="install-mysql-workbench-tool-to-connect-and-manage-mysql-database"></a>Instalar a ferramenta MySQL Workbench para conectar e gerenciar o banco de dados MySQL

1. Instalar a ferramenta **MySQL Workbench** na [página de downloads do MySQL](http://dev.mysql.com/downloads/windows/installer/)
2. Inicie o aplicativo e adicione clique no botão **Mysqlconnections +** para adicionar uma nova conexão. Use os dados da cadeia de conexão que você copiou do banco de dado MySQL do Azure criado anteriormente neste tutorial.
3. Depois de estabelecer a conexão, abra uma nova guia de **consulta** ; Cole os comandos de [MySQLIdentity. SQL](https://github.com/aspnet/samples/blob/master/samples/aspnet/Identity/AspNet.Identity.MySQL/MySQLIdentity.sql) na consulta e execute-os para criar as tabelas de banco de dados.
4. Agora você tem todas as ASP.NET Identity tabelas necessárias criadas em um banco de dados MySQL hospedado no Azure, conforme mostrado abaixo.  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image1.jpg)

## <a name="create-an-mvc-application-project-from-template-and-configure-it-to-use-mysql-provider"></a>Criar um projeto de aplicativo MVC a partir do modelo e configurá-lo para usar o provedor MySQL

Se necessário, instale o [Visual Studio Express 2013 para Web](https://go.microsoft.com/fwlink/?LinkId=299058) ou [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566) com a atualização 2.

### <a name="download-the-aspnetidentitymysql-project-from-github"></a>Baixar o projeto ASP. NET. Identity. MySQL do GitHub

1. Navegue até a URL do repositório em [ASPNET. Identity. MySQL (github)](https://github.com/aspnet/samples/tree/master/samples/aspnet/Identity/AspNet.Identity.MySQL/).
2. Baixe o código-fonte.
3. Extraia o arquivo. zip em uma pasta local.
4. Abra a solução AspNet. Identity. MySQL e Compile-a.

### <a name="create-a-new-mvc-application-project-from-template"></a>Criar um novo projeto de aplicativo MVC a partir do modelo

1. Clique com o botão direito do mouse na solução **ASPNET. Identity. MySQL** e **adicione**, **novo projeto**
2. Na caixa de diálogo **Adicionar novo projeto** , selecione  **C# Visual** à esquerda, em seguida, **Web** e, em seguida, selecione **aplicativo Web ASP.net**. Nomeie seu projeto **IdentityMySQLDemo**; e clique em OK.  
  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image2.jpg)
3. Na caixa de diálogo **novo projeto ASP.net** , selecione o modelo MVC com as opções padrão (que inclui **contas de usuário individuais** como método de autenticação) e clique em **OK**.![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image3.jpg)
4. Em Gerenciador de Soluções, clique com o botão direito do mouse no projeto IdentityMySQLDemo e selecione **gerenciar pacotes NuGet**. No diálogo Pesquisar caixa de texto, digite **Identity. EntityFramework**. Selecione este pacote na lista de resultados e clique em **desinstalar**. Será solicitado que você desinstale o EntityFramework do pacote de dependência. Clique em Sim, pois não haverá mais este pacote neste aplicativo.
5. Clique com o botão direito do mouse no projeto IdentityMySQLDemo, selecione **Adicionar**, **referência, solução, projetos;** selecione o projeto ASPNET. Identity. MySQL e clique em **OK**.
6. No projeto IdentityMySQLDemo, substitua todas as referências a  
    `using Microsoft.AspNet.Identity.EntityFramework;`  
   Com  
     `using AspNet.Identity.MySQL;`
7. Em IdentityModels.cs, defina **ApplicationDbContext** para derivar de **MySqlDatabase** e inclua um construtor que tenha um único parâmetro com o nome da conexão.  

    [!code-csharp[Main](implementing-a-custom-mysql-aspnet-identity-storage-provider/samples/sample1.cs)]
8. Abra o arquivo IdentityConfig.cs. No método **ApplicationUserManager. Create** , substitua a instanciação de usermanager pelo código a seguir:  

    [!code-csharp[Main](implementing-a-custom-mysql-aspnet-identity-storage-provider/samples/sample2.cs)]
9. Abra o arquivo Web. config e substitua a cadeia de caracteres DefaultConnection por essa entrada, substituindo os valores realçados pela cadeia de conexão do banco de dados MySQL criado nas etapas anteriores:  

    [!code-xml[Main](implementing-a-custom-mysql-aspnet-identity-storage-provider/samples/sample3.xml?highlight=2)]

## <a name="run-the-app-and-connect-to-the-mysql-db"></a>Execute o aplicativo e conecte-se ao banco de BD MySQL

1. Clique com o botão direito do mouse no projeto **IdentityMySQLDemo** e selecione **definir como projeto de inicialização**
2. Pressione **Ctrl + F5** para compilar e executar o aplicativo.
3. Clique na guia **registrar** na parte superior da página.
4. Insira um novo nome de usuário e senha e, em seguida, clique em **registrar**.  
  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image8.png)
5. O novo usuário agora está registrado e conectado.  
  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image9.png)
6. Volte para a ferramenta MySQL Workbench e inspecione o conteúdo da tabela **IdentityMySQLDatabase** . Inspecione a tabela usuários para as entradas ao registrar novos usuários.  
  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image10.png)

## <a name="next-steps"></a>Próximas etapas

Para obter mais informações sobre como habilitar outros métodos de autenticação neste aplicativo, consulte [criar um aplicativo ASP.NET MVC 5 com o Facebook e o Google OAuth2 e o logon OpenID](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md).

Para saber como integrar seu BD com o OAuth e configurar funções para limitar o acesso de usuários ao seu aplicativo, consulte [implantar um aplicativo Secure ASP.NET MVC 5 com associação, OAuth e banco de dados SQL no Azure](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data).
