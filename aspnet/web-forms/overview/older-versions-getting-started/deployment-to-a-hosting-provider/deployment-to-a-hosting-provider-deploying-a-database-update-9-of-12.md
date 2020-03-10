---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12
title: 'Implantando um aplicativo Web ASP.NET com SQL Server Compact usando o Visual Studio ou o Visual Web Developer: Implantando uma atualização de banco de dados-9 de 12 | Microsoft Docs'
author: tdykstra
description: Esta série de tutoriais mostra como implantar (publicar) um projeto de aplicativo Web ASP.NET que inclui um banco de dados SQL Server Compact usando o Visual Stu...
ms.author: riande
ms.date: 11/17/2011
ms.assetid: a8d776af-4735-4612-87f6-9f326587f2d3
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12
msc.type: authoredcontent
ms.openlocfilehash: 3385e1019d9e7a9263fd9513c39b25eb85febbb5
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78564438"
---
# <a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-deploying-a-database-update---9-of-12"></a>Implantando um aplicativo Web ASP.NET com SQL Server Compact usando o Visual Studio ou o Visual Web Developer: Implantando uma atualização de banco de dados-9 de 12

por [Tom Dykstra](https://github.com/tdykstra)

[Baixar o projeto inicial](https://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> Esta série de tutoriais mostra como implantar (publicar) um projeto de aplicativo Web ASP.NET que inclui um banco de dados SQL Server Compact usando o Visual Studio 2012 RC ou o Visual Studio Express 2012 RC para Web. Você também pode usar o Visual Studio 2010 se instalar a atualização de publicação na Web. Para obter uma introdução à série, consulte [o primeiro tutorial da série](deployment-to-a-hosting-provider-introduction-1-of-12.md).
> 
> Para ver um tutorial que mostra os recursos de implantação introduzidos após a versão RC do Visual Studio 2012, mostra como implantar SQL Server edições diferentes de SQL Server Compact e mostra como implantar o Azure App aplicativos Web do serviço, consulte [implantação da Web do ASP.NET usando o Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).

## <a name="overview"></a>Visão geral

Neste tutorial, você faz uma alteração de banco de dados e alterações de código relacionadas, testa as alterações no Visual Studio e, em seguida, implanta a atualização para os ambientes de teste e de produção.

Lembrete: se você receber uma mensagem de erro ou algo não funcionar enquanto percorre o tutorial, certifique-se de verificar a [página de solução de problemas](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md).

## <a name="adding-a-new-column-to-a-table"></a>Adicionando uma nova coluna a uma tabela

Nesta seção, você adiciona uma coluna data de nascimento à classe base `Person` para as entidades `Student` e `Instructor`. Em seguida, você atualiza a página que exibe dados do instrutor para que ele exiba a nova coluna.

No projeto *ContosoUniversity. Dal* , abra *Person.cs* e adicione a seguinte propriedade no final da classe `Person` (deve haver duas chaves de fechamento após a ti):

[!code-csharp[Main](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/samples/sample1.cs)]

Em seguida, atualize o método semente para que ele forneça um valor para a nova coluna. Abra *Migrations\Configuration.cs* e substitua o bloco de código que começa `var instructors = new List<Instructor>` com o seguinte bloco de código que inclui informações de data de nascimento:

[!code-csharp[Main](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/samples/sample2.cs)]

No projeto ContosoUniversity, abra *instrutores. aspx* e adicione um novo campo de modelo para exibir a data de nascimento. Adicione-o entre os itens para a data de contratação e a atribuição do Office:

[!code-aspx[Main](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/samples/sample3.aspx)]

(Se o recuo de código ficar fora de sincronia, você poderá pressionar CTRL-K e, em seguida, CTRL-D para reformatar o arquivo automaticamente.)

Compile a solução e, em seguida, abra a janela do **console do Gerenciador de pacotes** . Verifique se ContosoUniversity. DAL ainda está selecionado como o **projeto padrão**.

Na janela do **console do Gerenciador de pacotes** , selecione **ContosoUniversity. Dal** como o **projeto padrão**e, em seguida, digite o seguinte comando:

[!code-powershell[Main](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/samples/sample4.ps1)]

Quando esse comando for concluído, o Visual Studio abrirá o arquivo de classe que define a nova classe `DbMigration` e, no método `Up`, você poderá ver o código que cria a nova coluna.

![AddBirthDate_migration_code](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image1.png)

Compile a solução e, em seguida, digite o seguinte comando na janela do **console do Gerenciador de pacotes** (verifique se o projeto CONTOSOUNIVERSITY. Dal ainda está selecionado):

[!code-powershell[Main](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/samples/sample5.ps1)]

Quando o comando for concluído, execute o aplicativo e selecione a página instrutores. Quando a página for carregada, você verá que ela tem o novo campo data de nascimento.

[![Instructors_page_with_birth_date](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image3.png)](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image2.png)

## <a name="deploying-the-database-update-to-the-test-environment"></a>Implantando a atualização do banco de dados no ambiente de teste

Em **Gerenciador de soluções** selecione o projeto ContosoUniversity.

Na barra de ferramentas de **publicação de um clique da Web** , selecione o perfil de publicação de **teste** e clique em **publicar Web**. (Se a barra de ferramentas estiver desabilitada, selecione o projeto ContosoUniversity em **Gerenciador de soluções**.)

O Visual Studio implanta o aplicativo atualizado e o navegador é aberto na home page. Execute a página instrutores para verificar se a atualização foi implantada com êxito. Quando o aplicativo tenta acessar o banco de dados para essa página, Code First atualiza o esquema de banco de dados e executa o método `Seed`. Quando a página for exibida, você verá a coluna **data de nascimento** esperada com datas.

[![Instructors_page_with_birth_date_Test](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image5.png)](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image4.png)

## <a name="deploying-the-database-update-to-the-production-environment"></a>Implantando a atualização do banco de dados no ambiente de produção

Agora você pode implantar na produção. A única diferença é que você usará o *app\_offline. htm* para impedir que os usuários acessem o site e, portanto, atualizando o banco de dados enquanto estiver implantando as alterações. Para implantação de produção, execute as seguintes etapas:

- Carregue o arquivo *. htm do aplicativo\_offline* no site de produção.
- No Visual Studio, escolha o perfil de produção na barra de ferramentas de **publicação de um clique da Web** e clique em **publicar Web**.
- Exclua o arquivo *. htm do aplicativo\_offline* do site de produção.

> [!NOTE]
> Enquanto seu aplicativo está em uso no ambiente de produção, você deve implementar um plano de backup. Ou seja, você deve copiar periodicamente os arquivos *School-prod. sdf* e *ASPNET-prod. sdf* do site de produção para um local de armazenamento seguro, e deve manter várias gerações desses backups. Ao atualizar o banco de dados, você deve fazer uma cópia de backup imediatamente antes da alteração. Em seguida, se você cometer um erro e não o descobrir até depois de implantá-lo na produção, ainda poderá recuperar o banco de dados para o estado em que estava antes de ele ser corrompido.

Quando o Visual Studio abre a URL home page no navegador, a página o *aplicativo\_offline. htm* é exibida. Depois de excluir o arquivo *. htm do aplicativo\_offline* , você pode navegar até a sua Home Page novamente para verificar se a atualização foi implantada com êxito.

[![Instructors_page_with_birth_date_Prod](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image7.png)](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image6.png)

Agora você implantou uma atualização de aplicativo que incluiu uma alteração de banco de dados para teste e produção. O próximo tutorial mostra como migrar seu banco de dados do SQL Server Compact para SQL Server Express e SQL Server.

> [!div class="step-by-step"]
> [Anterior](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12.md)
> [Próximo](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12.md)
