---
uid: mvc/overview/older-versions-1/movie-database/create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs
title: Criar um aplicativo de banco de dados de filme em 15 minutosC#com o ASP.NET MVC () | Microsoft Docs
author: StephenWalther
description: Stephen Walther cria um aplicativo MVC totalmente controlado por banco de dados do ASP.NET do início ao fim. Este tutorial é uma ótima introdução para pessoas que são novas t...
ms.author: riande
ms.date: 01/27/2009
ms.assetid: dd1be137-91c5-47a8-8137-fecf0789c7f5
msc.legacyurl: /mvc/overview/older-versions-1/movie-database/create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs
msc.type: authoredcontent
ms.openlocfilehash: 1be5d135a44feb27626dd26a544b64cfb57b18a9
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74596032"
---
# <a name="create-a-movie-database-application-in-15-minutes-with-aspnet-mvc-c"></a>Criar um aplicativo de banco de dados de filme em 15 minutos com o ASP.NET MVC (C#)

por [Stephen Walther](https://github.com/StephenWalther)

[Código de download](https://download.microsoft.com/download/7/2/8/728F8794-E59A-4D18-9A56-7AD2DB05BD9D/MovieApp_CS.zip)

> Stephen Walther cria um aplicativo MVC totalmente controlado por banco de dados do ASP.NET do início ao fim. Este tutorial é uma ótima introdução para as pessoas que são novas no ASP.NET MVC Framework e que desejam ter uma noção do processo de criação de um aplicativo MVC ASP.NET.

A finalidade deste tutorial é dar uma noção de "o que é como" para criar um aplicativo MVC ASP.NET. Neste tutorial, eu gosto de criar um aplicativo MVC ASP.NET inteiro do início ao fim. Eu mostro como criar um aplicativo simples baseado em banco de dados que ilustra como você pode listar, criar e editar registros de banco de dados.

Para simplificar o processo de criação de nosso aplicativo, aproveitaremos os recursos do scaffolding do Visual Studio 2008. Vamos deixar o Visual Studio gerar o código inicial e o conteúdo para nossos controladores, modelos e exibições.

Se você trabalhou com Active Server páginas ou ASP.NET, você deve encontrar o ASP.NET MVC muito familiar. As exibições do MVC do ASP.NET são muito parecidas com as páginas em um aplicativo de páginas Active Server. E, assim como um aplicativo ASP.NET Web Forms tradicional, o ASP.NET MVC fornece acesso completo ao rico conjunto de linguagens e classes fornecidas pelo .NET Framework.

Minha esperança é que este tutorial lhe dará uma noção de como a experiência de criação de um aplicativo MVC ASP.NET é semelhante e diferente da experiência de criar uma página de Active Server ou ASP.NET aplicativo de Web Forms.

## <a name="overview-of-the-movie-database-application"></a>Visão geral do aplicativo de banco de dados de filme

Como nosso objetivo é manter as coisas simples, criaremos um aplicativo de banco de dados de filmes muito simples. Nosso aplicativo de banco de dados de filmes simples nos permitirá fazer três coisas:

1. Listar um conjunto de registros de banco de dados de filmes
2. Criar um novo registro de banco de dados de filme
3. Editar um registro de banco de dados de filme existente

Novamente, como queremos manter as coisas simples, aproveitaremos o número mínimo de recursos do ASP.NET MVC Framework necessário para criar nosso aplicativo. Por exemplo, não aproveitaremos o desenvolvimento controlado por testes.

Para criar nosso aplicativo, precisamos concluir cada uma das seguintes etapas:

1. Criar o projeto de aplicativo Web do ASP.NET MVC
2. Criar o banco de dados
3. Criar o modelo de banco de dados
4. Criar o controlador MVC ASP.NET
5. Criar as exibições do ASP.NET MVC

## <a name="preliminaries"></a>Etapas preliminares

Você precisará do Visual Studio 2008 ou do Visual Web Developer 2008 Express para criar um aplicativo MVC ASP.NET. Você também precisa baixar a estrutura MVC do ASP.NET.

Se você não possui o Visual Studio 2008, você pode baixar uma versão de avaliação de 90 dias do Visual Studio 2008 deste site:

[https://msdn.microsoft.com/vs2008/products/cc268305.aspx](https://msdn.microsoft.com/vs2008/products/cc268305.aspx)

Como alternativa, você pode criar aplicativos MVC ASP.NET com o Visual Web Developer Express 2008. Se você decidir usar o Visual Web Developer Express, deverá ter o Service Pack 1 instalado. Você pode baixar o Visual Web Developer 2008 Express com Service Pack 1 deste site:

[https://www.microsoft.com/downloads/details.aspx?FamilyId=BDB6391C-05CA-4036-9154-6DF4F6DEBD14&amp;d isplaylang = en](https://www.microsoft.com/downloads/details.aspx?FamilyId=BDB6391C-05CA-4036-9154-6DF4F6DEBD14&amp;displaylang=en)

Depois de instalar o Visual Studio 2008 ou o Visual Web Developer 2008, você precisa instalar o ASP.NET MVC Framework. Você pode baixar o ASP.NET MVC Framework no seguinte site:

[https://www.asp.net/mvc/](../../../index.md)

> [!NOTE] 
> 
> Em vez de baixar a estrutura ASP.NET e a estrutura MVC do ASP.NET individualmente, você pode aproveitar o Web Platform Installer. O Web Platform Installer é um aplicativo que permite que você gerencie facilmente os aplicativos instalados em seu computador:
> 
> [https://www.microsoft.com/web/gallery/Install.aspx](https://www.microsoft.com/web/gallery/Install.aspx)

## <a name="creating-an-aspnet-mvc-web-application-project"></a>Criando um projeto de aplicativo Web do ASP.NET MVC

Vamos começar criando um novo projeto de aplicativo Web ASP.NET MVC no Visual Studio 2008. Selecione o arquivo de opção de menu **, novo projeto** e você verá a caixa de diálogo novo projeto na Figura 1. Selecione C# como a linguagem de programação e selecione o modelo de projeto de aplicativo Web do ASP.NET MVC. Dê ao seu projeto o nome MovieApp e clique no botão OK.

[![caixa de diálogo novo projeto](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image1.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image1.png)

**Figura 01**: a caixa de diálogo novo projeto ([clique para exibir a imagem em tamanho normal](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image2.png))

Certifique-se de selecionar .NET Framework 3,5 na lista suspensa na parte superior da caixa de diálogo novo projeto ou o modelo de projeto de aplicativo Web ASP.NET MVC não aparecerá.

Sempre que você criar um novo projeto de aplicativo Web MVC, o Visual Studio solicitará que você crie um projeto de teste de unidade separado. A caixa de diálogo da Figura 2 é exibida. Como não criaremos testes neste tutorial devido a restrições de tempo (e, sim, devemos sentir um pouco culpado) Selecione a opção **não** e clique no botão **OK** .

> [!NOTE] 
> 
> O Visual Web Developer não dá suporte a projetos de teste.

[![caixa de diálogo novo projeto](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image2.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image3.png)

**Figura 02**: a caixa de diálogo Criar projeto de teste de unidade ([clique para exibir a imagem em tamanho normal](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image4.png))

Um aplicativo MVC ASP.NET tem um conjunto padrão de pastas: modelos, exibições e pastas de controladores. Você pode ver esse conjunto padrão de pastas na janela Gerenciador de Soluções. Precisaremos adicionar arquivos a cada uma das pastas modelos, exibições e controladores para criar nosso aplicativo de banco de dados de filmes.

Ao criar um novo aplicativo MVC com o Visual Studio, você obtém um aplicativo de exemplo. Como queremos começar do zero, precisamos excluir o conteúdo para este aplicativo de exemplo. Você precisa excluir o seguinte arquivo e a seguinte pasta:

- Controllers\HomeController.cs
- Views\Home

## <a name="creating-the-database"></a>Criando o banco de dados

Precisamos criar um banco de dados para manter os registros do banco de dados de filmes. Felizmente, o Visual Studio inclui um banco de dados gratuito chamado SQL Server Express. Siga estas etapas para criar o banco de dados:

1. Clique com o botão direito do mouse na pasta\_dados do aplicativo na janela Gerenciador de Soluções e selecione a opção de menu **Adicionar, novo item**.
2. Selecione a categoria de dados e selecione o modelo de banco de **dado** **SQL Server** (consulte a Figura 3).
3. Nomeie o novo banco de dados *MoviesDB. MDF* e clique no botão **Adicionar** .

Depois de criar o banco de dados, você pode se conectar ao banco de dados clicando duas vezes no arquivo MoviesDB. MDF localizado na pasta dados do aplicativo\_. Clicar duas vezes no arquivo MoviesDB. MDF abre a janela Gerenciador de Servidores.

> [!NOTE] 
> 
> A janela de Gerenciador de Servidores é chamada de janela de Gerenciador de Banco de Dados no caso do Visual Web Developer.

[![caixa de diálogo novo projeto](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image3.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image5.png)

**Figura 03**: Criando um banco de dados Microsoft SQL Server ([clique para exibir a imagem em tamanho normal](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image6.png))

Em seguida, precisamos criar uma nova tabela de banco de dados. De dentro da janela Gerenciador de servidores, clique com o botão direito do mouse na pasta tabelas e selecione a opção de menu **Adicionar nova tabela**. A seleção dessa opção de menu abre o designer de tabela de banco de dados. Crie as seguintes colunas de banco de dados:

<a id="0.1_table01"></a>

| **Nome da coluna** | **Tipo de dados** | **Permitir nulos** |
| --- | --- | --- |
| Id | Int | False |
| Cargo | Nvarchar (100) | False |
| Pasta | Nvarchar (100) | False |
| DateReleased | DateTime | False |

A primeira coluna, a coluna ID, tem duas propriedades especiais. Primeiro, você precisa marcar a coluna ID como a coluna de chave primária. Depois de selecionar a coluna ID, clique no botão **definir chave primária** (é o ícone que se parece com uma chave). Em segundo lugar, você precisa marcar a coluna ID como uma coluna de identidade. Na janela Propriedades de coluna, role para baixo até a seção especificação de identidade e expanda-a. Altere a propriedade **is Identity** para o valor **Yes**. Quando você terminar, a tabela deverá ser parecida com a Figura 4.

[![caixa de diálogo novo projeto](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image4.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image7.png)

**Figura 04**: a tabela de banco de dados de filmes ([clique para exibir a imagem em tamanho normal](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image8.png))

A etapa final é salvar a nova tabela. Clique no botão salvar (o ícone do disquete) e dê à nova tabela o nome filmes.

Depois de concluir a criação da tabela, adicione alguns registros de filme à tabela. Clique com o botão direito do mouse na tabela filmes na janela Gerenciador de Servidores e selecione a opção de menu **Mostrar dados da tabela**. Insira uma lista de seus filmes favoritos (consulte a Figura 5).

[![caixa de diálogo novo projeto](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image5.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image9.png)

**Figura 05**: inserindo registros de filme ([clique para exibir a imagem em tamanho normal](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image10.png))

## <a name="creating-the-model"></a>Criando o modelo

Em seguida, precisamos criar um conjunto de classes para representar nosso banco de dados. Precisamos criar um modelo de banco de dados. Aproveitaremos o Entity Framework da Microsoft para gerar automaticamente as classes para nosso modelo de banco de dados.

> [!NOTE] 
> 
> O ASP.NET MVC Framework não está vinculado ao Microsoft Entity Framework. Você pode criar suas classes de modelo de banco de dados aproveitando uma variedade de ferramentas de mapeamento relacional de objeto (ou/M), incluindo LINQ to SQL, Subsonic e NHibernate.

Siga estas etapas para iniciar o assistente de Modelo de Dados de Entidade:

1. Clique com o botão direito do mouse na pasta modelos na janela Gerenciador de Soluções e selecione a opção de menu **Adicionar, novo item**.
2. Selecione a categoria de **dados** e selecione o modelo de **modelo de dados de entidade ADO.net** .
3. Dê ao seu modelo de dados o nome *MoviesDBModel. edmx* e clique no botão **Adicionar** .

Depois que você clicar no botão Adicionar, o assistente de Modelo de Dados de Entidade será exibido (consulte a Figura 6). Siga estas etapas para concluir o assistente:

1. Na etapa **escolher conteúdo do modelo** , selecione a opção **gerar do banco de dados** .
2. Na etapa **escolher sua conexão de dados** , use a conexão de dados *MoviesDB. MDF* e o nome *MoviesDBEntities* para as configurações de conexão. Clique no botão **Avançar** .
3. Na etapa **escolher seus objetos de banco de dados** , expanda o nó tabelas, selecione a tabela filmes. Insira o namespace *MovieApp. Models* e clique no botão **concluir** .

[![caixa de diálogo novo projeto](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image6.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image11.png)

**Figura 06**: gerando um modelo de banco de dados com o assistente de modelo de dados de entidade ([clique para exibir a imagem em tamanho normal](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image12.png))

Depois de concluir o assistente de Modelo de Dados de Entidade, o designer de Modelo de Dados de Entidade é aberto. O designer deve exibir a tabela de banco de dados de filmes (veja a Figura 7).

[![caixa de diálogo novo projeto](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image7.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image13.png)

**Figura 07**: o Designer de modelo de dados de entidade ([clique para exibir a imagem em tamanho normal](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image14.png))

Precisamos fazer uma alteração antes de continuarmos. O assistente de dados de entidade gera uma classe de modelo denominada *filmes* que representa a tabela do banco de dados de filmes. Como usaremos a classe Movies para representar um filme específico, precisamos modificar o nome da classe como *filme* , em vez de *filmes* (singular, em vez de plural).

Clique duas vezes no nome da classe na superfície do designer e altere o nome da classe de filmes para filme. Depois de fazer essa alteração, clique no botão **salvar** (o ícone do disquete) para gerar a classe de filme.

## <a name="creating-the-aspnet-mvc-controller"></a>Criando o controlador MVC ASP.NET

A próxima etapa é criar o controlador MVC do ASP.NET. Um controlador é responsável por controlar como um usuário interage com um aplicativo MVC ASP.NET.

Siga estas etapas:

1. Na janela Gerenciador de Soluções, clique com o botão direito do mouse na pasta controladores e selecione a opção de menu **Adicionar, controlador**.
2. Na caixa de diálogo Adicionar controlador, insira o nome *HomeController* e marque a caixa de seleção **Adicionar métodos de ação para cenários de criação, atualização e detalhes** (consulte a Figura 8).
3. Clique no botão **Adicionar** para adicionar o novo controlador ao seu projeto.

Depois de concluir essas etapas, o controlador na Listagem 1 será criado. Observe que ele contém métodos nomeados index, Details, Create e Edit. Nas seções a seguir, adicionaremos o código necessário para que esses métodos funcionem.

[![caixa de diálogo novo projeto](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image8.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image15.png)

**Figura 08**: adicionando um novo controlador MVC ASP.net ([clique para exibir a imagem em tamanho normal](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image16.png))

**Listagem 1 – Controllers\HomeController.cs**

[!code-csharp[Main](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/samples/sample1.cs)]

## <a name="listing-database-records"></a>Listando registros de banco de dados

O método index () do controlador Home é o método padrão para um aplicativo MVC ASP.NET. Quando você executa um aplicativo MVC ASP.NET, o método index () é o primeiro método de controlador que é chamado.

Usaremos o método index () para exibir a lista de registros da tabela de banco de dados Movies. Aproveitaremos as classes de modelo de banco de dados que criamos anteriormente para recuperar os registros do banco de dados de filmes com o método index ().

Modifiquei a classe HomeController na Listagem 2 para que ela contenha um novo campo privado chamado \_DB. A classe MoviesDBEntities representa nosso modelo de banco de dados e usaremos essa classe para se comunicar com nosso banco de dados.

Também modifiquei o método index () na Listagem 2. O método index () usa a classe MoviesDBEntities para recuperar todos os registros de filme da tabela de banco de dados Movies. A expressão *\_DB. Movieset. ToList ()* retorna uma lista de todos os registros de filme da tabela de banco de dados de filmes.

A lista de filmes é passada para a exibição. Tudo o que é passado para o método View () é passado para a exibição como dados de exibição.

**Listagem 2 – controladores/HomeController. cs (método de índice modificado)**

[!code-csharp[Main](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/samples/sample2.cs)]

O método index () retorna uma exibição denominada index. Precisamos criar essa exibição para exibir a lista de registros do banco de dados de filmes. Siga estas etapas:

Você deve compilar seu projeto (selecione a opção de menu **Compilar, Compilar solução**) antes de abrir a caixa de diálogo **Adicionar exibição** ou nenhuma classe aparecerá na lista suspensa de classes de **dados de exibição** .

1. Clique com o botão direito do mouse no método index () no editor de código e selecione a opção de menu **Add View** (consulte a Figura 9).
2. Na caixa de diálogo Adicionar exibição, verifique se a caixa de seleção rotulada **criar uma exibição fortemente tipada** está marcada.
3. Na lista suspensa **Exibir conteúdo** , selecione a *lista*valor.
4. Na lista suspensa da **classe exibir dados** , selecione o valor *MovieApp. Models. Movie*.
5. Clique no botão Adicionar para criar o novo modo de exibição (veja a Figura 10).

Depois de concluir essas etapas, uma nova exibição denominada index. aspx será adicionada à pasta Views\Home. O conteúdo da exibição de índice está incluído na Listagem 3.

[![caixa de diálogo novo projeto](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image9.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image17.png)

**Figura 09**: adicionando uma exibição de uma ação do controlador ([clique para exibir a imagem em tamanho normal](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image18.png))

[![caixa de diálogo novo projeto](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image10.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image19.png)

**Figura 10**: criando uma nova exibição com a caixa de diálogo Adicionar exibição ([clique para exibir a imagem em tamanho normal](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image20.png))

**Listagem 3 – Views\Home\Index.aspx**

[!code-aspx[Main](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/samples/sample3.aspx)]

A exibição de índice exibe todos os registros de filme da tabela de banco de dados de filmes em uma tabela HTML. A exibição contém um loop foreach que itera em cada filme representado pela propriedade ViewData. Model. Se você executar o aplicativo pressionando a tecla F5, verá a página da Web na Figura 11.

[![caixa de diálogo novo projeto](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image11.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image21.png)

**Figura 11**: a exibição do índice ([clique para exibir a imagem em tamanho normal](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image22.png))

## <a name="creating-new-database-records"></a>Criando novos registros de banco de dados

A exibição de índice que criamos na seção anterior inclui um link para criar novos registros de banco de dados. Vamos continuar e implementar a lógica e criar a exibição necessária para criar novos registros de banco de dados de filmes.

O controlador inicial contém dois métodos chamados Create (). O primeiro método Create () não tem parâmetros. Essa sobrecarga do método Create () é usada para exibir o formulário HTML para criar um novo registro de banco de dados de filme.

O segundo método Create () tem um parâmetro FormCollection. Essa sobrecarga do método Create () é chamada quando o formulário HTML para criar um novo filme é Postado no servidor. Observe que esse segundo método Create () tem um atributo AcceptVerbs que impede que o método seja chamado, a menos que uma operação HTTP POST seja executada.

Esse segundo método Create () foi modificado na classe HomeController atualizada na Listagem 4. A nova versão do método Create () aceita um parâmetro de filme e contém a lógica para inserir um novo filme na tabela de banco de dados de filmes.

> [!NOTE] 
> 
> Observe o atributo BIND. Como não queremos atualizar a propriedade de ID do filme a partir do formulário HTML, precisamos excluir explicitamente essa propriedade.

**Listagem 4 – Controllers\HomeController.cs (método Create modificado)**

[!code-csharp[Main](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/samples/sample4.cs)]

O Visual Studio facilita a criação do formulário para a criação de um novo registro de banco de dados de filmes (consulte a Figura 12). Siga estas etapas:

1. Clique com o botão direito do mouse no método Create () no editor de código e selecione a opção de menu **Add View**.
2. Verifique se a caixa de seleção rotulada **criar uma exibição fortemente tipada** está marcada.
3. Na lista suspensa **Exibir conteúdo** , selecione o valor *criar*.
4. Na lista suspensa da **classe exibir dados** , selecione o valor *MovieApp. Models. Movie*.
5. Clique no botão **Adicionar** para criar o novo modo de exibição.

[![caixa de diálogo novo projeto](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image12.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image23.png)

**Figura 12**: adicionando o modo de exibição de criação ([clique para exibir a imagem em tamanho normal](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image24.png))

O Visual Studio gera a exibição na listagem 5 automaticamente. Essa exibição contém um formulário HTML que inclui campos que correspondem a cada uma das propriedades da classe Movie.

**Listagem 5 – Views\Home\Create.aspx**

[!code-aspx[Main](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/samples/sample5.aspx)]

> [!NOTE] 
> 
> O formulário HTML gerado pela caixa de diálogo Adicionar exibição gera um campo de formulário de ID. Como a coluna de ID é uma coluna de identidade, não precisamos desse campo de formulário e você pode removê-lo com segurança.

Depois de adicionar o modo de exibição criar, você pode adicionar novos registros de filme ao banco de dados. Execute o aplicativo pressionando a tecla F5 e clique no link criar novo para ver o formulário na Figura 13. Se você concluir e enviar o formulário, um novo registro de banco de dados de filme será criado.

Observe que você obtém a validação do formulário automaticamente. Se você não inserir uma data de lançamento para um filme ou inserir uma data de lançamento inválida, o formulário será exibido novamente e o campo data de lançamento será realçado.

[![caixa de diálogo novo projeto](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image13.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image25.png)

**Figura 13**: Criando um novo registro de banco de dados de filme ([clique para exibir a imagem em tamanho normal](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image26.png))

## <a name="editing-existing-database-records"></a>Editando registros de banco de dados existentes

Nas seções anteriores, discutimos como você pode listar e criar novos registros de banco de dados. Nesta seção final, discutiremos como você pode editar registros de banco de dados existentes.

Primeiro, precisamos gerar o formulário de edição. Essa etapa é fácil, pois o Visual Studio irá gerar o formulário de edição para nós automaticamente. Abra a classe HomeController.cs no editor de código do Visual Studio e siga estas etapas:

1. Clique com o botão direito do mouse no método Edit () no editor de código e selecione a opção de menu **Add View** (consulte a Figura 14).
2. Marque a caixa de seleção rotulada **criar uma exibição fortemente tipada**.
3. Na lista suspensa **Exibir conteúdo** , selecione o valor *Editar*.
4. Na lista suspensa da **classe exibir dados** , selecione o valor *MovieApp. Models. Movie*.
5. Clique no botão **Adicionar** para criar o novo modo de exibição.

A conclusão dessas etapas adiciona uma nova exibição denominada Edit. aspx à pasta Views\Home. Esta exibição contém um formulário HTML para editar um registro de filme.

[![caixa de diálogo novo projeto](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image14.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image27.png)

**Figura 14**: adicionando o modo de exibição de edição ([clique para exibir a imagem em tamanho normal](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image28.png))

> [!NOTE] 
> 
> O modo de exibição de edição contém um campo de formulário HTML que corresponde à propriedade de ID do filme. Como você não quer que as pessoas editem o valor da propriedade ID, remova esse campo de formulário.

Por fim, precisamos modificar o controlador Home para que ele dê suporte à edição de um registro de banco de dados. A classe HomeController atualizada está contida na Listagem 6.

**Listagem 6 – Controllers\HomeController.cs (métodos de edição)**

[!code-csharp[Main](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/samples/sample6.cs)]

Na Listagem 6, adicionei lógica adicional a ambas as sobrecargas do método Edit (). O primeiro método Edit () retorna o registro do banco de dados de filmes que corresponde ao parâmetro de ID passado para o método. A segunda sobrecarga executa as atualizações em um registro de filme no banco de dados.

Observe que você deve recuperar o filme original e, em seguida, chamar ApplyPropertyChanges () para atualizar o filme existente no banco de dados.

## <a name="summary"></a>Resumo

A finalidade deste tutorial era dar uma noção da experiência de criação de um aplicativo MVC ASP.NET. Espero que você tenha descoberto que a criação de um aplicativo Web ASP.NET MVC é muito semelhante à experiência de criação de uma página de Active Server ou de um aplicativo ASP.NET.

Neste tutorial, examinamos apenas os recursos mais básicos da estrutura MVC do ASP.NET. Em Tutoriais futuros, nos aprofundamos em tópicos como controladores, ações do controlador, exibições, dados de exibição e auxiliares HTML.

> [!div class="step-by-step"]
> [Avançar](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb.md)
