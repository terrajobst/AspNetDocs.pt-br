---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12
title: 'Implantando um aplicativo Web ASP.NET com o SQL Server Compact usando o Visual Studio ou o Visual Web Developer: Implantando uma atualização de banco de dados SQL Server-11 de 12 | Microsoft Docs'
author: tdykstra
description: Esta série de tutoriais mostra como implantar (publicar) um projeto de aplicativo Web ASP.NET que inclui um banco de dados SQL Server Compact usando o Visual Stu...
ms.author: riande
ms.date: 11/17/2011
ms.assetid: 5e2bb092-cb22-4511-ad0a-22ae12dd99b3
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12
msc.type: authoredcontent
ms.openlocfilehash: 0894c0ac24737e66b6960ef3d48aa17f78c6aa1d
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74621075"
---
# <a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-deploying-a-sql-server-database-update---11-of-12"></a>Implantando um aplicativo Web ASP.NET com o SQL Server Compact usando o Visual Studio ou o Visual Web Developer: Implantando uma atualização de banco de dados SQL Server-11 de 12

por [Tom Dykstra](https://github.com/tdykstra)

[Baixar o projeto inicial](https://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> Esta série de tutoriais mostra como implantar (publicar) um projeto de aplicativo Web ASP.NET que inclui um banco de dados SQL Server Compact usando o Visual Studio 2012 RC ou o Visual Studio Express 2012 RC para Web. Você também pode usar o Visual Studio 2010 se instalar a atualização de publicação na Web. Para obter uma introdução à série, consulte [o primeiro tutorial da série](deployment-to-a-hosting-provider-introduction-1-of-12.md).
> 
> Para obter um tutorial que mostra os recursos de implantação introduzidos após a versão RC do Visual Studio 2012, mostra como implantar SQL Server edições diferentes de SQL Server Compact e mostra como implantar em sites do Windows Azure, consulte [implantação da Web do ASP.NET usando o Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).

## <a name="overview"></a>{1&gt;Visão Geral&lt;1}

Este tutorial mostra como implantar uma atualização de banco de dados em um banco de dados SQL Server completo. Como Migrações do Code First faz todo o trabalho de atualizar o banco de dados, o processo é quase idêntico ao que você fez por SQL Server Compact no tutorial [implantando uma atualização de banco de dados](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12.md) .

Lembrete: se você receber uma mensagem de erro ou algo não funcionar enquanto percorre o tutorial, certifique-se de verificar a [página de solução de problemas](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md).

## <a name="adding-a-new-column-to-a-table"></a>Adicionando uma nova coluna a uma tabela

Nesta seção do tutorial, você fará uma alteração no banco de dados e as alterações de código correspondentes e os testará no Visual Studio na preparação para implantá-los nos ambientes de teste e produção. A alteração envolve a adição de uma coluna `OfficeHours` à entidade `Instructor` e a exibição das novas informações na página da Web dos **instrutores** .

No projeto ContosoUniversity. DAL, abra *Instructor.cs* e adicione a seguinte propriedade entre as propriedades `HireDate` e `Courses`:

[!code-csharp[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample1.cs)]

Atualize a classe do inicializador para que ela propaga a nova coluna com dados de teste. Abra *Migrations\Configuration.cs* e substitua o bloco de código que começa `var instructors = new List<Instructor>` com o seguinte bloco de código que inclui a nova coluna:

[!code-csharp[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample2.cs)]

No projeto ContosoUniversity, abra *instrutores. aspx* e adicione um novo campo de modelo para o horário comercial logo antes da marca de `</Columns>` de fechamento no primeiro controle `GridView`:

[!code-aspx[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample3.aspx)]

{1&gt;Compile a solução.&lt;1}

Abra a janela do **console do Gerenciador de pacotes** e selecione CONTOSOUNIVERSITY. Dal como o **projeto padrão**.

Insira os seguintes comandos:

[!code-powershell[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample4.ps1)]

Execute o aplicativo e selecione a página **instrutores** . A página leva um pouco mais tempo do que o normal para ser carregada, porque a Entity Framework recria o banco de dados e propaga-o com dado de teste.

[![Instructors_page_with_office_hours](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image2.png)](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image1.png)

## <a name="deploying-the-database-update-to-the-test-environment"></a>Implantando a atualização do banco de dados no ambiente de teste

Quando você usa Migrações do Code First, o método para implantar uma alteração de banco de dados em SQL Server é o mesmo que para o SQL Server Compact. No entanto, você precisa alterar o perfil de publicação de teste porque ele ainda está configurado para migrar de SQL Server Compact para SQL Server.

A primeira etapa é remover as transformações de cadeia de conexão que você criou no tutorial anterior. Elas não são mais necessárias porque você especificará as transformações de cadeia de conexão no perfil de publicação, como fez antes de configurar a guia **pacote/publicar SQL** para migração para o SQL Server.

Abra o arquivo *Web. Test. config* e remova o elemento `connectionStrings`. A única transformação restante no arquivo *Web. Test. config* é para o valor `Environment` no elemento `appSettings`.

Agora você pode atualizar o perfil de publicação e publicar no ambiente de teste.

Abra o assistente **publicar Web** e, em seguida, alterne para a guia **perfil** .

Selecione o perfil de publicação de **teste** .

Selecione a guia **Configurações**.

Clique em **habilitar novos aprimoramentos de publicação de banco de dados**.

Na caixa Cadeia de conexão para **SchoolContext**, insira o mesmo valor que você usou no arquivo de transformação *Web. Test. config* no tutorial anterior:

[!code-console[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample5.cmd)]

Selecione **executar migrações do Code First (é executado no início do aplicativo)** . (Em sua versão do Visual Studio, a caixa de seleção pode ser rotulada como **aplicar migrações do Code First**.)

Na caixa Cadeia de conexão para **DefaultConnection**, insira o mesmo valor que você usou no arquivo de transformação *Web. Test. config* no tutorial anterior:

[!code-console[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample6.cmd)]

Deixe o **banco de dados de atualização** limpo.

Clique em **Publicar**.

O Visual Studio implanta as alterações de código no ambiente de teste e abre o navegador para a Contoso University home page.

Selecione a página instrutores.

Quando o aplicativo executa essa página, ele tenta acessar o banco de dados. Migrações do Code First verifica se o banco de dados está atualizado e descobre que a migração mais recente ainda não foi aplicada. Migrações do Code First aplica a migração mais recente, executa o método `Seed` e, em seguida, a página é executada normalmente. Você vê a nova coluna de horários do Office com os dados propagados.

[![Instructors_page_with_OfficeHours_Test](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image4.png)](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image3.png)

## <a name="deploying-the-database-update-to-the-production-environment"></a>Implantando a atualização do banco de dados no ambiente de produção

Também é necessário alterar o perfil de publicação para o ambiente de produção. Nesse caso, você removerá o perfil existente e criará um novo importando um arquivo. publishsettings atualizado. O arquivo atualizado incluirá a cadeia de conexão para o banco de dados SQL Server em Cytanium.

Como vimos quando você implantou o no ambiente de teste, você não precisa mais de transformações de cadeia de conexão no arquivo de transformação *Web. Production. config* . Abra esse arquivo e remova o elemento `connectionStrings`. As transformações restantes são para o valor `Environment` no elemento `appSettings` e o elemento `location` que restringe o acesso aos relatórios de erros do ELMAH.

Antes de criar um novo perfil de publicação para produção, baixe um arquivo. publishsettings atualizado da mesma maneira que você fez anteriormente no tutorial [implantando no ambiente de produção](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md) . (No painel de controle Cytanium, clique em **sites**e, em seguida, clique no site da **contosouniversity.com** . Selecione a guia **publicação na Web** e clique em **baixar perfil de publicação para este site**.) O motivo pelo qual você está fazendo isso é pegar a cadeia de conexão do banco de dados no arquivo. publishsettings. A cadeia de conexão não estava disponível na primeira vez que você baixou o arquivo, pois você ainda estava usando SQL Server Compact e não havia criado o banco de dados SQL Server em Cytanium ainda.

Agora você pode atualizar o perfil de publicação e publicar no ambiente de produção.

Abra o assistente **publicar Web** e, em seguida, alterne para a guia **perfil** .

Clique em **gerenciar perfis**e, em seguida, exclua o perfil de produção.

Feche o assistente **publicar Web** para salvar essa alteração.

Abra o assistente **publicar Web** novamente e clique em **importar**.

Na guia **conexão** , altere a **URL de destino** para o valor apropriado se você estiver usando uma URL temporária.

Clique em **Avançar**.

Na guia **configurações** , clique em **habilitar novos aprimoramentos de publicação de banco de dados**.

Na lista suspensa cadeia de conexão para **SchoolContext**, selecione a cadeia de conexão Cytanium.

![Selecting_Cytanium_connection_string](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image5.png)

Selecione **executar Code First migrações (é executado no início do aplicativo)** .

Na lista suspensa cadeia de conexão para **DefaultConnection**, selecione a cadeia de conexão Cytanium.

Selecione a guia **perfil** , clique em **gerenciar perfis**e renomeie o perfil de "contosouniversity.com-implantação da Web" como "produção".

Feche o perfil de publicação para salvar a alteração e, em seguida, abra-a novamente.

Clique em **Publicar**. (Para um site de produção real, copie o *aplicativo\_offline. htm* para produção e coloque-o em sua pasta de projeto antes de publicar e, em seguida, remova-o quando a implantação for concluída.)

O Visual Studio implanta as alterações de código no ambiente de teste e abre o navegador para a Contoso University home page.

Selecione a página instrutores.

Migrações do Code First atualiza o banco de dados da mesma maneira que fazia no ambiente de teste. Você vê a nova coluna de horários do Office com os dados propagados.

![Instructors_page_with_OfficeHours_Prod](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image6.png)

Agora você implantou com êxito uma atualização de aplicativo que incluiu uma alteração de banco de dados, usando um banco de dados SQL Server.

## <a name="more-information"></a>Mais Informações

Isso conclui esta série de tutoriais sobre como implantar um aplicativo Web ASP.NET em um provedor de Hospedagem de terceiros. Para obter mais informações sobre qualquer um dos tópicos abordados nesses tutoriais, consulte o [mapa de conteúdo de implantação do ASP.net](https://msdn.microsoft.com/library/bb386521(v=vs.110).aspx) no site do MSDN.

## <a name="acknowledgements"></a>Agradecimentos

Gostaria de agradecer às seguintes pessoas que fizeram contribuições significativas para o conteúdo desta série de tutoriais:

- [Alberto Poblacion, MVP &amp; MCT, Espanha](https://mvp.support.microsoft.com/profile/Alberto)
- Jarod Ferguson, MVP de desenvolvimento de plataforma de dados, Estados Unidos
- Adverso do Mittal, Microsoft
- [Kristina Olson, Microsoft](https://blogs.iis.net/krolson/default.aspx)
- [Mike Papa, Microsoft](http://www.mikepope.com/blog/DisplayBlog.aspx)
- Mohit Srivastava, Microsoft
- [Raffaele Rialdi, Itália](http://www.iamraf.net/)
- [Rick Anderson, Microsoft](https://blogs.msdn.com/b/rickandy/)
- [Sayed Hashimi, Microsoft](http://sedodream.com/default.aspx)(twitter: [@sayedihashimi](http://twitter.com/sayedihashimi))
- [Scott Hanselman](http://www.hanselman.com/blog/) (twitter: [@shanselman](http://twitter.com/shanselman))
- [Scott Hunter, Microsoft](https://blogs.msdn.com/b/scothu/) (twitter: [@coolcsh](http://twitter.com/coolcsh))
- [Srđan Božović, Sérvia](http://msforge.net/blogs/zmajcek/)
- [Vishal Joshi, Microsoft](http://vishaljoshi.blogspot.com/) (twitter: [@vishalrjoshi](http://twitter.com/vishalrjoshi))

> [!div class="step-by-step"]
> [Anterior](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12.md)
> [Próximo](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md)
