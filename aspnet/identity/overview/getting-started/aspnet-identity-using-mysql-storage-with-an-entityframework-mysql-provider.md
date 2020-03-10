---
uid: identity/overview/getting-started/aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider
title: 'ASP.NET Identity: usando o armazenamento do MySQL com um provedor deC#MySQL do EntityFramework ()-ASP.NET 4. x'
author: maumar
description: Este tutorial mostra como substituir o mecanismo de armazenamento de dados padrão para ASP.NET Identity com o EntityFramework (provedor do cliente SQL) com um MySQL provid...
ms.author: riande
ms.date: 12/10/2013
ms.assetid: 15253312-a92c-43ba-908e-b5dacd3d08b8
ms.custom: seoapril2019
msc.legacyurl: /identity/overview/getting-started/aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider
msc.type: authoredcontent
ms.openlocfilehash: e89ed139657c5ce9ddcc56879946c62038919483
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78584003"
---
# <a name="aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider-c"></a>ASP.NET Identity: usando o armazenamento do MySQL com um provedor deC#MySQL do EntityFramework ()

por [Maurycy Markowski](https://github.com/maumar), [Raquel Soares de Almeida](https://github.com/raquelsa), [Robert McMurray](https://github.com/rmcmurray)

> Este tutorial mostra como substituir o mecanismo de armazenamento de dados padrão para [**ASP.net Identity**](introduction-to-aspnet-identity.md) com o EntityFramework (provedor do cliente SQL) com um provedor MySQL.

Os tópicos a seguir serão abordados neste tutorial:

- Criando um banco de dados MySQL no Azure
- Criando um aplicativo MVC usando Visual Studio 2013 modelo MVC
- Configurando o EntityFramework para trabalhar com um provedor de banco de dados MySQL
- Executando o aplicativo para verificar os resultados

No final deste tutorial, você terá um aplicativo MVC com a ASP.NET Identity Store que está usando um banco de dados MySQL hospedado no Azure.

## <a name="creating-a-mysql-database-instance-on-azure"></a>Criando uma instância de banco de dados MySQL no Azure

1. Faça logon no [Portal do Azure](https://go.microsoft.com/fwlink/?linkid=529715&amp;clcid=0x409).
2. Clique em **novo** na parte inferior da página e, em seguida, selecione **armazenar**:

    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image2.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image1.png)
3. No assistente **escolher e complemento** , selecione banco de **dados MySQL ClearDB**e clique na seta **Avançar** na parte inferior do quadro:

   [Clique na imagem a seguir para expandi-la. ] [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image4.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image3.png)
4. Mantenha o plano **gratuito** padrão, altere o **nome** para **IdentityMySQLDatabase**, selecione a região mais próxima de você e clique na seta **Avançar** na parte inferior do quadro:

   [Clique na imagem a seguir para expandi-la. ] [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image6.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image5.png)
5. Clique na marca de seleção de **compra** para concluir a criação do banco de dados.

   [Clique na imagem a seguir para expandi-la. ] [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image8.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image7.png)
6. Depois que o banco de dados tiver sido criado, você pode gerenciá-lo na guia **COMPLEMENTOS** no portal de gerenciamento. Para recuperar as informações de conexão do banco de dados, clique em **informações de conexão** na parte inferior da página:

   [Clique na imagem a seguir para expandi-la. ] [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image10.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image9.png)
7. Copie a cadeia de conexão clicando no botão Copiar pelo campo **CONNECTIONSTRING** e salve-a; Você usará essas informações posteriormente neste tutorial para seu aplicativo MVC:

   [Clique na imagem a seguir para expandi-la. ] [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image12.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image11.png)

## <a name="creating-an-mvc-application-project"></a>Criando um projeto de aplicativo MVC

Para concluir as etapas nesta seção do tutorial, primeiro será necessário instalar [Visual Studio Express 2013 para Web](https://go.microsoft.com/fwlink/?LinkId=299058) ou [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566). Depois que o Visual Studio tiver sido instalado, use as seguintes etapas para criar um novo projeto de aplicativo MVC:

1. Abra o Visual Studio 2103.
2. Clique em **novo projeto** na página **inicial** , ou você pode clicar no menu **arquivo** e em **novo projeto**:

   [Clique na imagem a seguir para expandi-la. ] [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image2.jpg)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image1.jpg)
3. Quando a caixa de diálogo **novo projeto** for exibida, expanda **Visual C#**  na lista de modelos, clique em **Web**e selecione **aplicativo Web ASP.net**. Nomeie seu projeto **IdentityMySQLDemo** e clique em **OK**:

   [Clique na imagem a seguir para expandi-la. ] [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image14.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image13.png)
4. Na caixa de diálogo **novo projeto ASP.net** , selecione o **MVC** templatewith as opções padrão; Isso irá configurar **contas de usuário individuais** como o método de autenticação. Clique em **OK**:

   [Clique na imagem a seguir para expandi-la. ] [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image16.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image15.png)

## <a name="configure-entityframework-to-work-with-a-mysql-database"></a>Configurar o EntityFramework para funcionar com um banco de dados MySQL

### <a name="update-the-entity-framework-assembly-for-your-project"></a>Atualizar o assembly Entity Framework para seu projeto

O aplicativo MVC criado a partir do modelo de Visual Studio 2013 contém uma referência ao pacote do [EntityFramework 6.0.0](http://www.nuget.org/packages/EntityFramework) , mas houve atualizações para esse assembly desde sua versão, que contêm melhorias significativas de desempenho. Para usar essas atualizações mais recentes em seu aplicativo, use as etapas a seguir.

1. Abra seu projeto MVC no Visual Studio.
2. Clique em **ferramentas**, em **Gerenciador de pacotes NuGet**e em **console do Gerenciador de pacotes**:

   [Clique na imagem a seguir para expandi-la. ] [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image18.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image17.png)
3. O **console do Gerenciador de pacotes** aparecerá na seção inferior do Visual Studio. Digite &quot;**Update-Package EntityFramework**&quot; e pressione ENTER:

   [Clique na imagem a seguir para expandi-la. ] [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image20.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image19.png)

### <a name="install-the-mysql-provider-for-entityframework"></a>Instalar o provedor MySQL para o EntityFramework

Para que o EntityFramework se conecte ao banco de dados MySQL, você precisa instalar um provedor MySQL. Para fazer isso, abra o **console do Gerenciador de pacotes** e digite &quot;**install-Package MySQL. Data. Entity-pre**&quot;e pressione Enter.

> [!NOTE]
> Esta é uma versão de pré-lançamento do assembly e, como tal, pode conter bugs. Você não deve usar uma versão de pré-lançamento do provedor em produção.

[Clique na imagem a seguir para expandi-la.]

[![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image22.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image21.png)

### <a name="making-project-configuration-changes-to-the-webconfig-file-for-your-application"></a>Fazendo alterações de configuração do projeto no arquivo Web. config para seu aplicativo

Nesta seção, você configurará o Entity Framework para usar o provedor MySQL que acabou de instalar, registrar a fábrica do provedor MySQL e adicionar a cadeia de conexão do Azure.

> [!NOTE]
> Os exemplos a seguir contêm uma versão de assembly específica para MySql. Data. dll. Se a versão do assembly for alterada, será necessário modificar as definições de configuração apropriadas com a versão correta.

1. Abra o arquivo Web. config para seu projeto no Visual Studio 2013.
2. Localize as seguintes definições de configuração, que definem o provedor de banco de dados padrão e a fábrica para o Entity Framework:

    [!code-xml[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample1.xml)]
3. Substitua esses parâmetros de configuração pelo seguinte, que irá configurar o Entity Framework para usar o provedor MySQL:

    [!code-xml[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample2.xml)]
4. Localize a seção &lt;connectionStrings&gt; e substitua-a pelo código a seguir, que definirá a cadeia de conexão para o banco de dados MySQL que está hospedado no Azure (Observe que o valor providerName também foi alterado do original):

    [!code-xml[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample3.xml?highlight=3-4)]

### <a name="adding-custom-migrationhistory-context"></a>Adicionando contexto de MigrationHistory personalizado

Entity Framework Code First usa uma tabela **MigrationHistory** para controlar as alterações de modelo e garantir a consistência entre o esquema de banco de dados e o esquema conceitual. No entanto, essa tabela não funciona para o MySQL por padrão porque a chave primária é muito grande. Para corrigir essa situação, será necessário reduzir o tamanho da chave para essa tabela. Para fazer isso, execute as seguintes etapas:

1. As informações de esquema para esta tabela são capturadas em um **HistoryContext**, que pode ser modificado como qualquer outro **DbContext**. Para fazer isso, adicione um novo arquivo de classe chamado **MySqlHistoryContext.cs** ao projeto e substitua seu conteúdo pelo código a seguir:

    [!code-csharp[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample4.cs)]
2. Em seguida, você precisará configurar Entity Framework para usar o **HistoryContext**modificado, em vez do padrão um. Isso pode ser feito aproveitando os recursos de configuração baseada em código. Para fazer isso, adicione um novo arquivo de classe chamado **MySqlConfiguration.cs** ao seu projeto e substitua seu conteúdo por:

    [!code-csharp[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample5.cs)]

### <a name="creating-a-custom-entityframework-initializer-for-applicationdbcontext"></a>Criando um inicializador do EntityFramework personalizado para ApplicationDbContext

O provedor MySQL que é apresentado neste tutorial atualmente não oferece suporte a Entity Framework migrações, portanto, você precisará usar inicializadores de modelo para se conectar ao banco de dados. Como este tutorial está usando uma instância do MySQL no Azure, será necessário criar um inicializador de Entity Framework personalizado.

> [!NOTE]
> Essa etapa não será necessária se você estiver se conectando a uma instância do SQL Server no Azure ou se estiver usando um banco de dados hospedado no local.

Para criar um inicializador de Entity Framework personalizado para MySQL, use as seguintes etapas:

1. Adicione um novo arquivo de classe chamado **MySqlInitializer.cs** ao projeto e substitua o conteúdo pelo código a seguir:

    [!code-csharp[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample6.cs?highlight=23)]
2. Abra o arquivo **IdentityModels.cs** do seu projeto, localizado no diretório **modelos** , e substitua o conteúdo pelo seguinte:

    [!code-csharp[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample7.cs)]

## <a name="running-the-application-and-verifying-the-database"></a>Executando o aplicativo e verificando o banco de dados

Depois de concluir as etapas nas seções anteriores, você deve testar seu banco de dados. Para fazer isso, execute as seguintes etapas:

1. Pressione **Ctrl + F5** para compilar e executar o aplicativo Web.
2. Clique na guia **registrar** na parte superior da página:

   [Clique na imagem a seguir para expandi-la. ] [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image4.jpg)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image3.jpg)
3. Insira um novo nome de usuário e senha e, em seguida, clique em **registrar**:

   [Clique na imagem a seguir para expandi-la. ] [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image24.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image23.png)
4. Neste ponto, as tabelas de ASP.NET Identity são criadas no banco de dados MySQL e o usuário é registrado e conectado ao aplicativo:

   [Clique na imagem a seguir para expandi-la. ] [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image6.jpg)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image5.jpg)

### <a name="installing-mysql-workbench-tool-to-verify-the-data"></a>Instalando a ferramenta MySQL Workbench para verificar os dados

1. Instalar a ferramenta **MySQL Workbench** na [página de downloads do MySQL](http://dev.mysql.com/downloads/windows/installer/)
2. Na guia Assistente de instalação: **seleção de recursos** , selecione **MySQL Workbench** na seção **aplicativos** .
3. Inicie o aplicativo e adicione uma nova conexão usando os dados da cadeia de conexão do banco de dados MySQL do Azure que você criou no implorando deste tutorial.
4. Depois de estabelecer a conexão, inspecione as tabelas de **ASP.net Identity** criadas no **IdentityMySQLDatabase.**
5. Você verá que todas as tabelas ASP.NET Identity necessárias são criadas conforme mostrado na imagem abaixo:

   [Clique na imagem a seguir para expandi-la. ] [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image8.jpg)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image7.jpg)
6. Inspecione a tabela **aspnetusers** por instância para verificar as entradas ao registrar novos usuários.

   [Clique na imagem a seguir para expandi-la. ] [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image26.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image25.png)
