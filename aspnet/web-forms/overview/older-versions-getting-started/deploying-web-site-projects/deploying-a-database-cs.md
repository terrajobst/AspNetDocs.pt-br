---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/deploying-a-database-cs
title: Implantando um bancoC#de dados () | Microsoft Docs
author: rick-anderson
description: Implantar um aplicativo Web ASP.NET envolve a obtenção dos arquivos e recursos necessários do ambiente de desenvolvimento para o ambiente de produção. Para da...
ms.author: riande
ms.date: 04/23/2009
ms.assetid: ff537a10-9f1f-43fe-9bcb-3dda161ba8f5
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/deploying-a-database-cs
msc.type: authoredcontent
ms.openlocfilehash: 83657be794e1ea31f6ad2f2b4adc274724d60cf2
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78638204"
---
# <a name="deploying-a-database-c"></a>Implantação de um banco de dados (C#)

por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixar código](https://download.microsoft.com/download/E/6/F/E6FE3A1F-EE3A-4119-989A-33D1A9F6F6DD/ASPNET_Hosting_Tutorial_07_CS.zip) ou [baixar PDF](https://download.microsoft.com/download/C/3/9/C391A649-B357-4A7B-BAA4-48C96871FEA6/aspnet_tutorial07_DeployDB_cs.pdf)

> Implantar um aplicativo Web ASP.NET envolve a obtenção dos arquivos e recursos necessários do ambiente de desenvolvimento para o ambiente de produção. Para aplicativos da Web controlados por dados, isso inclui o esquema de banco de dados e o Data. Este tutorial é o primeiro de uma série que explora as etapas necessárias para implantar com êxito o banco de dados do ambiente de desenvolvimento para produção.

## <a name="introduction"></a>Introdução

Implantar um aplicativo Web ASP.NET envolve a obtenção dos arquivos e recursos necessários do ambiente de desenvolvimento para o ambiente de produção. No decorrer dos seis últimos tutoriais, examinamos a implantação de um aplicativo Web simples de revisões de livros. Este site de demonstração foi composto por vários recursos do lado do servidor – ASP.NET páginas, arquivos de configuração, um arquivo `Web.sitemap` e assim por diante, juntamente com os recursos do lado do cliente, como imagens e arquivos CSS. Mas e quanto aos aplicativos Web controlados por dados? Quais etapas adicionais devem ser seguidas para implantar um aplicativo Web que usa um banco de dados?

Ao longo dos vários tutoriais, abordaremos as etapas necessárias para implantar um aplicativo Web controlado por dados. Este tutorial começa examinando como obter o conteúdo e o esquema de um banco de dados do ambiente de desenvolvimento para o ambiente de produção, enquanto o tutorial subsequente analisa as alterações de configuração necessárias. A seguir, exploraremos os desafios da implantação de um banco de dados que usa o Serviços de Aplicativos (Associação, funções, perfil e assim por diante).

## <a name="examining-the-updated-book-reviews-web-application"></a>Examinando o aplicativo Web de análises de livros atualizados

Para demonstrar a implantação de um aplicativo Web controlado por dados, eu atualizei o aplicativo Web do livro de um site estático simples para um controlado por dados. Como antes, há duas versões do aplicativo neste tutorial download: uma que usa o modelo de projeto de aplicativo Web e outra que usa o modelo de projeto de site.

O aplicativo Web Reviews de livro atualizado usa um banco de dados [SQL Server 2008 Express Edition](https://www.microsoft.com/express/sql/default.aspx) , que é armazenado na pasta s `App_Data` do site (`~/App_Data/Reviews.mdf`). Se você tiver SQL Server 2008 instalado em seu computador, a demonstração deverá ser executada sem erros. Se você tiver uma versão mais antiga do SQL Server poderá instalar gratuitamente o SQL Server 2008 Express Edition ou pode usar os scripts de banco de dados disponíveis neste tutorial s download para criar o banco de dados por conta própria.

O banco de dados `Reviews.mdf` contém quatro tabelas:

- `Genres`-inclui um registro para cada gênero, como tecnologia, ficção e negócios.
- `Books`-inclui um registro para cada revisão, com colunas como `Title`, `GenreId`, `ReviewDate`e `Review`, entre outras.
- `Authors`-inclui informações sobre cada autor que contribuiu para um livro revisado.
- `BooksAuthors`-uma tabela de junção muitos para muitos que especifica quais autores escreveram quais livros.

A Figura 1 mostra um diagrama ER dessas quatro tabelas.

[![o banco de dados dos aplicativos Web do aplicativo de análise é composto de quatro tabelas](deploying-a-database-cs/_static/image2.jpg)](deploying-a-database-cs/_static/image1.jpg) 

**Figura 1**: o livro o banco de dados do aplicativo Web é composto de quatro tabelas ([clique para exibir a imagem em tamanho normal](deploying-a-database-cs/_static/image3.jpg))

A versão anterior do livro revisa o site da Web tinha uma página ASP.NET separada para cada livro. Por exemplo, havia uma página chamada `~/Tech/TYASP35.aspx` que continha a revisão para *ensinar ASP.NET 3,5 em 24 horas*. Essa nova versão controlada por dados do site tem as revisões armazenadas no banco de dado e uma única página ASP.NET, examine. aspx? ID =*BookID*, que exibe a revisão do livro especificado. Da mesma forma, existe uma página gênero. aspx? ID =*gêneroid* que lista os livros revisados no gênero especificado.

As figuras 2 e 3 mostram as páginas `Genre.aspx` e `Review.aspx` em ação. Observe a URL na barra de endereços de cada página. Na Figura 2, ele é gênero. aspx? ID = 85d164ba-1123-4c47-82a0-c8ec75de7e0e. Como 85d164ba-1123-4c47-82a0-c8ec75de7e0e é o valor `GenreId` para o gênero Technology, o título s da página lê "revisões de tecnologia" e a lista com marcadores enumera essas revisões no site que se enquadram nesse gênero.

[![a página de gênero de tecnologia](deploying-a-database-cs/_static/image5.jpg)](deploying-a-database-cs/_static/image4.jpg) 

**Figura 2**: a página de gênero da tecnologia ([clique para exibir a imagem em tamanho normal](deploying-a-database-cs/_static/image6.jpg))

[![a revisão para ensinar a ASP.NET 3,5 em 24 horas](deploying-a-database-cs/_static/image8.jpg)](deploying-a-database-cs/_static/image7.jpg) 

**Figura 3**: a revisão para *ensinar-se ASP.net 3,5 em 24 horas* ([clique para exibir a imagem em tamanho normal](deploying-a-database-cs/_static/image9.jpg))

O livro Reviews do aplicativo Web também inclui uma seção de administração em que os administradores podem adicionar, editar e excluir gêneros, revisões e informações de autor. Atualmente, qualquer visitante pode acessar a seção Administração. Em um tutorial futuro, adicionaremos suporte para contas de usuário e só permitirá que usuários autorizados nas páginas de administração.

Se você baixar o aplicativo de revisões de livros, tenha em mente que seu objetivo é demonstrar a implantação de um aplicativo controlado por dados. Ele não exibe as práticas recomendadas no que diz respeito ao design do aplicativo. Por exemplo, não há nenhuma camada de acesso a dados separada (DAL); as páginas ASP.NET se comunicam diretamente com o banco de dados por meio do controle SqlDataSource ou do código ADO.NET em suas classes code-behind. Para obter uma visão mais detalhada sobre a criação de aplicativos controlados por dados usando uma arquitetura em camadas, consulte meus [tutoriais *de trabalho com dados* ](../../data-access/index.md).

## <a name="databases-on-development-versus-production"></a>Bancos de dados no desenvolvimento versus produção

Quando você inicia o desenvolvimento em um aplicativo Web controlado por dados, você deve especificar uma cadeia de conexão de banco de dado, que fornece os detalhes do aplicativo sobre como se conectar ao banco de dados. Essa cadeia de conexão especifica, entre outras coisas, o servidor de banco de dados, o nome do banco de dados e as informações de segurança. Geralmente, o banco de dados usado pelo aplicativo durante o desenvolvimento é diferente do banco de dados usado quando ele está em produção. Há muitos benefícios em usar bancos de dados diferentes para desenvolvimento versus produção. Ter um banco de dados diferente no desenvolvimento significa que você não precisa se preocupar com a modificação acidental ou a exclusão do Live Data. Ele também permite que você coloque dados de teste fictícios ou faça alterações significativas no modelo de dados sem precisar se preocupar com os efeitos no aplicativo em produção. A desvantagem de ter um banco de dados diferente nos ambientes de desenvolvimento e produção é que, quando o aplicativo é implantado, o banco de dados e todas as alterações pertinentes no esquema ou no banco de dados são também devem ser implantados.

Antes da primeira implantação, há apenas uma instância do banco de dados e essa instância está no ambiente de desenvolvimento. Ao implantar o aplicativo para produção pela primeira vez, não só devemos copiar os arquivos necessários do lado do servidor e do cliente, mas também copiar o banco de dados do ambiente de desenvolvimento para o ambiente de produção. É aí que agora temos o aplicativo Web de análises de livros – o banco de dados reside na pasta `App_Data` em nosso ambiente de desenvolvimento, mas ainda não foi enviado para o ambiente de produção.

Depois que o aplicativo tiver sido implantado, haverá duas cópias do banco de dados. À medida que o aplicativo amadurece, novos recursos podem ser adicionados, exigindo uma alteração no modelo de dados (como adicionar novas colunas a tabelas existentes, fazer alterações em colunas existentes, adicionar novas tabelas e assim por diante). Quando o aplicativo Web é implantado pela próxima vez, as alterações aplicadas ao banco de dados no ambiente de desenvolvimento desde a última implantação devem ser aplicadas ao banco de dados de produção. Algumas estratégias para gerenciar esse processo são discutidas em um tutorial futuro. Este tutorial concentra-se na implantação de todo o banco de dados do ambiente de desenvolvimento para produção.

## <a name="deploying-the-database-to-the-production-environment"></a>Implantando o banco de dados no ambiente de produção

O restante deste tutorial analisa como implantar o banco de dados do ambiente de desenvolvimento no ambiente de produção. Se você estiver acompanhando, precisará certificar-se de que sua conta com o provedor de host da Web inclui Microsoft SQL Server suporte a banco de dados. Você também precisará ter algumas informações em mãos, ou seja, o nome do servidor de banco de dados, o nome do banco de dados e o nome de usuário e a senha usados para se conectar ao banco de dados.

Conforme observado anteriormente neste tutorial, o livro banco de dados do site s é um banco de dados SQL Server 2008 Express Edition armazenado na pasta `App_Data`. Seria um motivo para que a implantação desse tipo de banco de dados fosse tão simples quanto copiar a pasta `App_Data` do ambiente de desenvolvimento para o ambiente de produção. No entanto, a maioria dos provedores de host da Web não oferece suporte à Hospedagem de bancos de dados na pasta `App_Data` por causa de motivos de segurança. Em vez disso, os hosts da Web fornecem uma conta em um servidor de banco de dados SQL Server em seu ambiente. A implantação do banco de dados do ambiente de desenvolvimento no ambiente de produção requer a obtenção do seu banco de dados registrado no servidor de banco de dados do host Web.

Então, como você obtém o banco de dados do ambiente de desenvolvimento para o ambiente de produção? Há algumas maneiras de fazer isso dependendo de quais serviços seu host Web oferece. Com alguns hosts, como DiscountASP.NET, você pode fazer o FTP de um backup do banco de dados ou do arquivo de `.mdf` real para seu site e, em seguida, no painel de controle, restaurar o arquivo de backup ou anexar o arquivo de `.mdf` ao servidor de banco de dados SQL Server. Com essas ferramentas, implantar o banco de dados é tão simples quanto copiar a pasta `App_Data` para o ambiente de produção e, em seguida, anexá-la por meio do painel de controle. Talvez essa seja a maneira mais fácil e rápida de publicar seu banco de dados pela primeira vez.

Outra abordagem é usar o assistente de publicação de banco de dados. O assistente de publicação de banco de dados é um aplicativo de área de trabalho do Windows que irá gerar os comandos SQL para criar o esquema de s do banco de dados-as tabelas, procedimentos armazenados, exibições, funções definidas pelo usuário e assim por diante – e, opcionalmente, os dados em suas tabelas. Em seguida, você pode se conectar ao servidor de banco de dados do provedor de host Web por meio de SQL Server Management Studio e executar esse script para duplicar o banco de dados em produção. Melhor ainda, se o provedor de host da Web oferecer suporte aos [serviços de publicação de banco de dados](http://www.codeplex.com/sqlhost/Wiki/View.aspx?title=Database%20Publishing%20Services&amp;referringTitle=Home) da Microsoft, você poderá ter o script gerado pelo assistente de publicação de banco de dados executado automaticamente no servidor de banco de dados em seu nome. Como o assistente de publicação de banco de dados gera um script que cria o esquema de banco de dados e os mesmos, ele funcionará independentemente de o provedor de host da Web oferecer recursos como anexar um arquivo de `.mdf` carregado.

### <a name="generating-the-sql-commands-to-create-the-database-schema-and-data-using-the-database-publishing-wizard"></a>Gerando os comandos SQL para criar o esquema de banco de dados e o usando o assistente de publicação de banco

Vamos examinar usando o assistente de publicação de banco de dados para implantar o banco de dados de revisões de livros em produção. Se você estiver usando o Visual Studio 2008 ou posterior, o assistente de publicação de banco de dados já estará instalado. Se você estiver usando o Visual Studio 2005, será necessário primeiro [baixar e instalar](https://www.microsoft.com/downloads/details.aspx?familyid=56E5B1C5-BF17-42E0-A410-371A838E570A&amp;displaylang=en) o assistente.

Abra o Visual Studio e navegue até o banco de dados `Reviews.mdf`. Se você estiver usando o Visual Web Developer, vá para a Gerenciador de Banco de Dados; Se você estiver usando o Visual Studio, use o Gerenciador de Servidores. A Figura 4 mostra o banco de dados `Reviews.mdf` no Gerenciador de Banco de Dados no Visual Web Developer. Como mostra a Figura 4, o banco de dados `Reviews.mdf` é composto de quatro tabelas, três procedimentos armazenados e uma função definida pelo usuário.

[![localizar o banco de dados no Gerenciador de Banco de Dados ou Gerenciador de Servidores](deploying-a-database-cs/_static/image11.jpg)](deploying-a-database-cs/_static/image10.jpg) 

**Figura 4**: Localize o banco de dados no Gerenciador de Banco de Dados ou Gerenciador de servidores ([clique para exibir a imagem em tamanho normal](deploying-a-database-cs/_static/image12.jpg))

Clique com o botão direito do mouse no nome do banco de dados e escolha a opção "publicar no provedor" no menu de contexto. Isso inicia o assistente de publicação de banco de dados (consulte a Figura 5). Clique em avançar para avançar a tela inicial.

[![tela inicial do assistente de publicação de banco de dados](deploying-a-database-cs/_static/image14.jpg)](deploying-a-database-cs/_static/image13.jpg) 

**Figura 5**: tela inicial do assistente de publicação de banco de dados ([clique para exibir a imagem em tamanho normal](deploying-a-database-cs/_static/image15.jpg))

A segunda tela do assistente lista os bancos de dados acessíveis ao assistente de publicação de banco de dados e permite que você escolha entre criar scripts de todos os objetos no banco de dados selecionado ou escolher quais objetos criar script. Selecione o banco de dados apropriado e deixe a opção "gerar script de todos os objetos no banco de dados selecionado" marcada.

> [!NOTE]
> Se você receber o erro "não há objetos no banco de dados *DatabaseName* dos tipos programáveis por este assistente" ao clicar em avançar na tela mostrada na Figura 6, verifique se o caminho para o arquivo de banco de dados não é muito longo. Foi descoberto que esse erro pode ocorrer se o caminho para o arquivo de banco de dados for muito longo.

[![tela inicial do assistente de publicação de banco de dados](deploying-a-database-cs/_static/image17.jpg)](deploying-a-database-cs/_static/image16.jpg) 

**Figura 6**: tela inicial do assistente de publicação de banco de dados ([clique para exibir a imagem em tamanho normal](deploying-a-database-cs/_static/image18.jpg))

Na próxima tela, você pode gerar um arquivo de script ou, se o seu host da Web oferecer suporte a ele, publique o banco de dados diretamente no servidor de banco de dados do provedor de host Web. Como mostra a Figura 7, estou tendo o script gravado no arquivo `C:\REVIEWS.MDF.sql`.

[![script do banco de dados para um arquivo ou publicá-lo diretamente no provedor de host da Web](deploying-a-database-cs/_static/image20.jpg)](deploying-a-database-cs/_static/image19.jpg) 

**Figura 7**: gerar script do banco de dados para um arquivo ou publicá-lo diretamente no provedor de host da Web ([clique para exibir a imagem em tamanho normal](deploying-a-database-cs/_static/image21.jpg))

A tela subsequente solicita uma variedade de opções de script. Você pode especificar se o script deve incluir instruções DROP para remover esses objetos existentes. O padrão é true, o que é bom ao implantar um banco de dados pela primeira vez. Você também pode especificar se o banco de dados de destino é SQL Server 2000, SQL Server 2005 ou SQL Server 2008. Por fim, você pode indicar se deseja criar o script do esquema e dos dados, apenas dos dados ou apenas do esquema. O esquema é a coleção de objetos de banco de dados, as tabelas, os procedimentos armazenados, os modos de exibição e assim por diante. Os dados são as informações que residem nas tabelas.

Como a Figura 8 ilustra, eu tenho o assistente configurado para descartar objetos de banco de dados existentes, para gerar script para um banco de dados SQL Server 2008 e para publicar o esquema e o dado.

[![especificar as opções de publicação](deploying-a-database-cs/_static/image23.jpg)](deploying-a-database-cs/_static/image22.jpg) 

**Figura 8**: especificar as opções de publicação ([clique para exibir a imagem em tamanho normal](deploying-a-database-cs/_static/image24.jpg))

As duas telas finais resumem as ações que estão prestes a ser tomadas e exibem o status do script. O resultado líquido da execução do assistente é que temos um arquivo de script que contém os comandos SQL necessários para criar o banco de dados na produção e preenchê-lo com os mesmos dados do desenvolvimento.

### <a name="executing-the-sql-commands-on-the-production-environment-database"></a>Executando os comandos SQL no banco de dados do ambiente de produção

Agora que temos o script que contém os comandos do SQL para criar o banco de dados e os respectivos, tudo o que resta é executar o script no banco de dados de produção. Alguns provedores de host da Web oferecem uma caixa de texto em seu painel de controle, na qual você pode inserir comandos SQL para executar em seu banco de dados. Se você tiver um arquivo de script muito grande, essa opção poderá não funcionar (o arquivo de script `REVIEWS.MDF.sql` tem mais de 425 KB de tamanho, por exemplo).

Uma abordagem melhor é conectar-se diretamente ao servidor de banco de dados de produção usando o SQL Server Management Studio (SSMS). Se você tiver uma edição não Express do SQL Server instalado em seu computador, provavelmente já terá o SSMS instalado. Caso contrário, você pode [baixar e instalar](https://www.microsoft.com/downloads/details.aspx?FamilyId=C243A5AE-4BD1-4E3D-94B8-5A0F62BF7796&amp;displaylang=en) uma cópia gratuita do SQL Server Management Studio Express Edition.

Inicie o SSMS e conecte-se ao servidor de banco de dados do host Web usando as informações fornecidas pelo provedor de host da Web.

[![conectar-se ao servidor de banco de dados do provedor de host Web](deploying-a-database-cs/_static/image26.jpg)](deploying-a-database-cs/_static/image25.jpg) 

**Figura 9**: conectar-se ao servidor de banco de dados do provedor de host da Web ([clique para exibir a imagem em tamanho normal](deploying-a-database-cs/_static/image27.jpg))

Expanda a guia bancos de dados e localize seu banco de dados. Clique no botão Nova consulta no canto superior esquerdo da barra de ferramentas, Cole os comandos SQL do arquivo de script criado pelo assistente de publicação de banco de dados e clique no botão Executar para executar esses comandos no servidor do banco de dados de produção. Se o arquivo de script for especialmente grande, pode levar vários minutos para executar os comandos.

[![conectar-se ao servidor de banco de dados do provedor de host Web](deploying-a-database-cs/_static/image29.jpg)](deploying-a-database-cs/_static/image28.jpg) 

**Figura 10**: conectar-se ao servidor de banco de dados do provedor de host da Web ([clique para exibir a imagem em tamanho normal](deploying-a-database-cs/_static/image30.jpg))

Isso é tudo! Neste ponto, o banco de dados de desenvolvimento foi duplicado para produção. Se você atualizar o banco de dados no SSMS, deverá ver os novos objetos de banco de dados. A Figura 11 mostra as tabelas, os procedimentos armazenados e as funções definidas pelo usuário do banco de dados de produção, que espelham aquelas no banco de dados de desenvolvimento. E, como instruímos o assistente de publicação de banco de dados para publicar o dado, as tabelas s do banco de dados de produção têm os mesmos dados que as tabelas de banco de dado de desenvolvimento no momento em que o assistente foi executado. A Figura 12 mostra os dados na tabela `Books` no banco de dados de produção.

[![os objetos de banco de dados foram duplicados no banco de dados de produção](deploying-a-database-cs/_static/image32.jpg)](deploying-a-database-cs/_static/image31.jpg) 

**Figura 11**: os objetos de banco de dados foram duplicados no banco de dados de produção ([clique para exibir a imagem em tamanho normal](deploying-a-database-cs/_static/image33.jpg))

[![o banco de dados de produção contém os mesmos dados que no banco de dado de desenvolvimento](deploying-a-database-cs/_static/image35.jpg)](deploying-a-database-cs/_static/image34.jpg) 

**Figura 12**: o Database de produção contém os mesmos dados que no banco de dado de desenvolvimento ([clique para exibir a imagem em tamanho normal](deploying-a-database-cs/_static/image36.jpg))

Neste ponto, implantamos apenas o banco de dados de desenvolvimento em produção. Ainda não examinamos a implantação do próprio aplicativo Web ou examinamos quais alterações de configuração são necessárias para que o aplicativo na produção use o banco de dados de produção. Abordaremos esses problemas no próximo tutorial!

## <a name="summary"></a>Resumo

A implantação de um aplicativo Web controlado por dados requer a cópia do Database usado durante o desenvolvimento para o ambiente de produção. Muitos provedores de host da Web oferecem ferramentas para simplificar o processo de implantação de um banco de dados. Por exemplo, com DiscountASP.NET, você pode fazer o FTP de seu banco de dados `.mdf` arquivo (ou um backup) e, em seguida, anexar o banco de dados ao servidor de banco de dados no painel de controle. Outra opção que funciona independentemente de quais recursos seu provedor de host da Web oferece é a ferramenta Microsoft s Publishing Database, que gera um script de comandos SQL para criar o esquema e os dados do banco de dado de desenvolvimento. Depois que esse script tiver sido gerado, você poderá executá-lo no banco de dados de produção.

Agora que o livro revisar o banco de dados do aplicativo Web está em produção, podemos implantar o aplicativo. No entanto, as informações de configuração do aplicativo Web especificam a cadeia de conexão para o banco de dados e essa cadeia de conexão faz referência ao banco de dados de desenvolvimento. Precisamos atualizar essas informações de cadeia de conexão ao implantar o site para produção. O próximo tutorial analisa essas diferenças de configuração e percorre as etapas necessárias para publicar o site de revisões de livros controlados por dados para produção.

Boa programação!

#### <a name="further-reading"></a>Leitura adicional

Para obter mais informações sobre os tópicos discutidos neste tutorial, consulte os seguintes recursos:

- [Baixe o assistente de publicação de banco de dados Microsoft SQL Server 1,1](https://www.microsoft.com/downloads/details.aspx?familyid=56E5B1C5-BF17-42E0-A410-371A838E570A&amp;displaylang=en)
- [Baixe o Microsoft SQL Server Management Studio Express Edition](https://www.microsoft.com/downloads/details.aspx?FamilyId=C243A5AE-4BD1-4E3D-94B8-5A0F62BF7796&amp;displaylang=en)

> [!div class="step-by-step"]
> [Anterior](core-differences-between-iis-and-the-asp-net-development-server-cs.md)
> [Próximo](configuring-the-production-web-application-to-use-the-production-database-cs.md)
