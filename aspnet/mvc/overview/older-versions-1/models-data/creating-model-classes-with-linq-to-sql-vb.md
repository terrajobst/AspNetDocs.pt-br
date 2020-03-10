---
uid: mvc/overview/older-versions-1/models-data/creating-model-classes-with-linq-to-sql-vb
title: Criando classes de modelo com LINQ to SQL (VB) | Microsoft Docs
author: microsoft
description: O objetivo deste tutorial é explicar um método de criação de classes de modelo para um aplicativo MVC ASP.NET. Neste tutorial, você aprenderá a criar o modelo c...
ms.author: riande
ms.date: 10/07/2008
ms.assetid: a4a25a75-d71f-4509-98b4-df72e748985a
msc.legacyurl: /mvc/overview/older-versions-1/models-data/creating-model-classes-with-linq-to-sql-vb
msc.type: authoredcontent
ms.openlocfilehash: 88a5f1037d93ef3bdc95bf60b6005ebb254ab440
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78581266"
---
# <a name="creating-model-classes-with-linq-to-sql-vb"></a>Criação de classes de modelo com o LINQ to SQL (VB)

pela [Microsoft](https://github.com/microsoft)

[Baixar PDF](https://download.microsoft.com/download/1/1/f/11f721aa-d749-4ed7-bb89-a681b68894e6/ASPNET_MVC_Tutorial_10_VB.pdf)

> O objetivo deste tutorial é explicar um método de criação de classes de modelo para um aplicativo MVC ASP.NET. Neste tutorial, você aprenderá a criar classes de modelo e a executar o acesso ao banco de dados aproveitando o LINQ to SQL da Microsoft.

O objetivo deste tutorial é explicar um método de criação de classes de modelo para um aplicativo MVC ASP.NET. Neste tutorial, você aprenderá a criar classes de modelo e a executar o acesso ao banco de dados aproveitando o LINQ to SQL da Microsoft.

Neste tutorial, criamos um aplicativo de banco de dados de filme básico. Começamos criando o aplicativo de banco de dados de filmes da maneira mais rápida e fácil possível. Executamos todo o nosso acesso a dados diretamente de nossas ações do controlador.

Em seguida, você aprenderá a usar o padrão de repositório. Usar o padrão de repositório requer um pouco mais de trabalho. No entanto, a vantagem de adotar esse padrão é que ele permite que você crie aplicativos que sejam adaptáveis a serem alterados e possam ser facilmente testados.

## <a name="what-is-a-model-class"></a>O que é uma classe de modelo?

Um modelo MVC contém toda a lógica do aplicativo que não está contida em uma exibição MVC ou controlador MVC. Em particular, um modelo MVC contém toda a lógica de acesso de dados e de negócios do seu aplicativo.

Você pode usar uma variedade de tecnologias diferentes para implementar a lógica de acesso a dados. Por exemplo, você pode criar suas classes de acesso a dados usando as classes Microsoft Entity Framework, NHibernate, Subsonic ou ADO.NET.

Neste tutorial, uso LINQ to SQL para consultar e atualizar o banco de dados. LINQ to SQL fornece um método muito fácil de interagir com um banco de dados Microsoft SQL Server. No entanto, é importante entender que o ASP.NET MVC Framework não está vinculado a LINQ to SQL de nenhuma forma. O ASP.NET MVC é compatível com qualquer tecnologia de acesso a dados.

## <a name="create-a-movie-database"></a>Criar um banco de dados de filmes

Neste tutorial, para ilustrar como você pode criar classes de modelo, criamos um aplicativo de banco de dados de filme simples. A primeira etapa é criar um novo banco de dados. Clique com o botão direito do mouse na pasta\_dados do aplicativo na janela Gerenciador de Soluções e selecione a opção de menu **Adicionar, novo item**. Selecione o modelo de banco de dados SQL Server, dê a ele o nome MoviesDB. MDF e clique no botão **Adicionar** (veja a Figura 1).

[![adicionar um novo banco de dados SQL Server](creating-model-classes-with-linq-to-sql-vb/_static/image2.png)](creating-model-classes-with-linq-to-sql-vb/_static/image1.png)

**Figura 01**: adicionando um novo banco de dados SQL Server ([clique para exibir a imagem em tamanho normal](creating-model-classes-with-linq-to-sql-vb/_static/image3.png))

Após criar o novo banco de dados, você pode abrir o banco de dados clicando duas vezes no arquivo MoviesDB. MDF na pasta de dados do aplicativo\_. Clicar duas vezes no arquivo MoviesDB. MDF abre a janela Gerenciador de Servidores (consulte a Figura 2).

|   | A janela de Gerenciador de Servidores é chamada de janela de Gerenciador de Banco de Dados ao usar o Visual Web Developer. |
|---|----------------------------------------------------------------------------------------------------|
|   |                                                                                                    |

[![usando a janela Gerenciador de Servidores](creating-model-classes-with-linq-to-sql-vb/_static/image5.png)](creating-model-classes-with-linq-to-sql-vb/_static/image4.png)

**Figura 02**: usando a janela de Gerenciador de servidores ([clique para exibir a imagem em tamanho normal](creating-model-classes-with-linq-to-sql-vb/_static/image6.png))

Precisamos adicionar uma tabela ao nosso banco de dados que representa nossos filmes. Clique com o botão direito do mouse na pasta tabelas e selecione a opção de menu **Adicionar nova tabela**. A seleção dessa opção de menu abre a Designer de Tabela (consulte a Figura 3).

[![usando a janela Gerenciador de Servidores](creating-model-classes-with-linq-to-sql-vb/_static/image8.png)](creating-model-classes-with-linq-to-sql-vb/_static/image7.png)

**Figura 03**: a designer de tabela ([clique para exibir a imagem em tamanho normal](creating-model-classes-with-linq-to-sql-vb/_static/image9.png))

Precisamos adicionar as seguintes colunas à nossa tabela de banco de dados:

| **Nome da Coluna** | **Tipo de Dados** | **Permitir Nulos** |
| --- | --- | --- |
| ID | Int | Falso |
| Title | Nvarchar(200) | Falso |
| Diretor | Nvarchar(50) | Falso |

Você precisa fazer duas coisas especiais para a coluna ID. Primeiro, você precisa marcar a coluna ID como uma coluna de chave primária selecionando a coluna na Designer de Tabela e clicando no ícone de uma chave. LINQ to SQL exige que você especifique as colunas de chave primária ao executar inserções ou atualizações no banco de dados.

Em seguida, você precisa marcar a coluna ID como uma coluna de identidade atribuindo o valor Sim à propriedade **is Identity** (consulte a Figura 3). Uma coluna de identidade é uma coluna que recebe um novo número automaticamente sempre que você adiciona uma nova linha de dados a uma tabela.

Depois de fazer essas alterações, salve a tabela com o nome tblMovie. Você pode salvar a tabela clicando no botão salvar.

## <a name="create-linq-to-sql-classes"></a>Criar classes de LINQ to SQL

Nosso modelo MVC conterá LINQ to SQL classes que representam a tabela de banco de dados tblMovie. A maneira mais fácil de criar essas classes de LINQ to SQL é clicar com o botão direito do mouse na pasta modelos, selecionar **Adicionar, novo item**, selecionar o modelo classes de LINQ to SQL, atribuir o nome Movie. dbml e clicar no botão **Adicionar** (consulte a Figura 4).

[![criar classes de LINQ to SQL](creating-model-classes-with-linq-to-sql-vb/_static/image11.png)](creating-model-classes-with-linq-to-sql-vb/_static/image10.png)

**Figura 04**: Criando classes de LINQ to SQL ([clique para exibir a imagem em tamanho normal](creating-model-classes-with-linq-to-sql-vb/_static/image12.png))

Imediatamente depois de criar o filme LINQ to SQL classes, o Object Relational Designer é exibido. Você pode arrastar tabelas de banco de dados da janela Gerenciador de Servidores para a Object Relational Designer para criar LINQ to SQL classes que representam tabelas de banco de dados específicas. Precisamos adicionar a tabela de banco de dados tblMovie à Object Relational Designer (consulte a Figura 4).

[![usando o Object Relational Designer](creating-model-classes-with-linq-to-sql-vb/_static/image14.png)](creating-model-classes-with-linq-to-sql-vb/_static/image13.png)

**Figura 05**: usando o Object Relational Designer ([clique para exibir a imagem em tamanho normal](creating-model-classes-with-linq-to-sql-vb/_static/image15.png))

Por padrão, o Object Relational Designer cria uma classe com o mesmo nome da tabela de banco de dados que você arrasta para o designer. No entanto, não queremos chamar nossa classe tblMovie. Portanto, clique no nome da classe no designer e altere o nome da classe para filme.

Por fim, lembre-se de clicar no botão **salvar** (a imagem do disquete) para salvar as Classes de LINQ to SQL. Caso contrário, as classes de LINQ to SQL não serão geradas pelo Object Relational Designer.

## <a name="using-linq-to-sql-in-a-controller-action"></a>Usando LINQ to SQL em uma ação do controlador

Agora que temos nossas classes de LINQ to SQL, podemos usar essas classes para recuperar dados do banco de dado. Nesta seção, você aprenderá a usar LINQ to SQL classes diretamente dentro de uma ação do controlador. Vamos exibir a lista de filmes da tabela de banco de dados tblMovies em uma exibição MVC.

Primeiro, precisamos modificar a classe HomeController. Essa classe pode ser encontrada na pasta controladores do seu aplicativo. Modifique a classe para que ela se pareça com a classe na Listagem 1.

**Listagem 1 – `Controllers\HomeController.vb`**

[!code-vb[Main](creating-model-classes-with-linq-to-sql-vb/samples/sample1.vb)]

A ação index () na Listagem 1 usa um LINQ to SQL classe DataContext (o MovieDataContext) para representar o banco de dados MoviesDB. A classe MoveDataContext foi gerada pelo Object Relational Designer do Visual Studio.

Uma consulta LINQ é executada em relação ao DataContext para recuperar todos os filmes da tabela de banco de dados tblMovies. A lista de filmes é atribuída a uma variável local chamada filmes. Por fim, a lista de filmes é passada para a exibição por meio de dados de exibição.

Para mostrar os filmes, precisamos modificar a exibição do índice. Você pode encontrar a exibição de índice na pasta Views\Home\ Atualize a exibição de índice para que ela se pareça com a exibição na Listagem 2.

**Listagem 2 – `Views\Home\Index.aspx`**

[!code-aspx[Main](creating-model-classes-with-linq-to-sql-vb/samples/sample2.aspx)]

Observe que a exibição de índice modificada inclui uma diretiva &lt;% @ Import namespace%&gt; na parte superior da exibição. Essa diretiva importa o namespace MvcApplication1. Precisamos desse namespace para trabalhar com as classes de modelo – em particular, a classe Movie – na exibição.

A exibição na Listagem 2 contém um loop for each que itera em todos os itens representados pela propriedade ViewData. Model. O valor da propriedade Title é exibido para cada filme.

Observe que o valor da propriedade ViewData. Model é convertido em um IEnumerable. Isso é necessário para executar um loop pelo conteúdo de ViewData. Model. Outra opção aqui é criar uma exibição fortemente tipada. Quando você cria uma exibição fortemente tipada, você converte a propriedade ViewData. Model em um tipo específico na classe code-behind de uma exibição.

Se você executar o aplicativo depois de modificar a classe HomeController e a exibição de índice, obterá uma página em branco. Você obterá uma página em branco porque não há nenhum registro de filme na tabela de banco de dados tblMovies.

Para adicionar registros à tabela de banco de dados tblMovies, clique com o botão direito do mouse na tabela de banco de dados tblMovies na janela Gerenciador de Servidores (Gerenciador de Banco de Dados janela no Visual Web Developer) e selecione a opção de menu **Mostrar dados da tabela**. Você pode inserir registros de filme usando a grade exibida (consulte a Figura 5).

[![inserindo filmes](creating-model-classes-with-linq-to-sql-vb/_static/image17.png)](creating-model-classes-with-linq-to-sql-vb/_static/image16.png)

**Figura 06**: inserindo filmes ([clique para exibir a imagem em tamanho normal](creating-model-classes-with-linq-to-sql-vb/_static/image18.png))

Depois de adicionar alguns registros de banco de dados à tabela tblMovies e executar o aplicativo, você verá a página na Figura 7. Todos os registros do banco de dados de filmes são exibidos em uma lista com marcadores.

[![exibir filmes com a exibição de índice](creating-model-classes-with-linq-to-sql-vb/_static/image20.png)](creating-model-classes-with-linq-to-sql-vb/_static/image19.png)

**Figura 07**: exibindo filmes com a exibição de índice ([clique para exibir a imagem em tamanho normal](creating-model-classes-with-linq-to-sql-vb/_static/image21.png))

## <a name="using-the-repository-pattern"></a>Usando o padrão de repositório

Na seção anterior, usamos LINQ to SQL classes diretamente dentro de uma ação do controlador. Usamos a classe MovieDataContext diretamente da ação do controlador de índice (). Não há nada de errado com isso no caso de um aplicativo simples. No entanto, trabalhar diretamente com LINQ to SQL em uma classe de controlador cria problemas quando você precisa criar um aplicativo mais complexo.

O uso de LINQ to SQL dentro de uma classe de controlador dificulta a troca de tecnologias de acesso a dados no futuro. Por exemplo, você pode optar por mudar do uso do Microsoft LINQ to SQL para usar o Entity Framework da Microsoft como sua tecnologia de acesso a dados. Nesse caso, você precisaria reescrever todos os controladores que acessam o banco de dados em seu aplicativo.

O uso de LINQ to SQL dentro de uma classe de controlador também dificulta a criação de testes de unidade para seu aplicativo. Normalmente, você não deseja interagir com um banco de dados ao executar testes de unidade. Você deseja usar os testes de unidade para testar a lógica do aplicativo e não o servidor de banco de dados.

Para criar um aplicativo MVC que seja mais adaptável para futuras alterações e que possa ser testado com mais facilidade, você deve considerar o uso do padrão Repository. Ao usar o padrão de repositório, você cria uma classe de repositório separada que contém toda a lógica de acesso ao banco de dados.

Ao criar a classe Repository, você cria uma interface que representa todos os métodos usados pela classe Repository. Em seus controladores, você escreve seu código na interface em vez do repositório. Dessa forma, você pode implementar o repositório usando tecnologias de acesso a dados diferentes no futuro.

A interface na Listagem 3 é denominada IMovieRepository e representa um único método chamado ListAll ().

**Listagem 3 – `Models\IMovieRepository.vb`**

[!code-vb[Main](creating-model-classes-with-linq-to-sql-vb/samples/sample3.vb)]

A classe Repository na Listagem 4 implementa a interface IMovieRepository. Observe que ele contém um método chamado ListAll () que corresponde ao método exigido pela interface IMovieRepository.

**Listagem 4 – `Models\MovieRepository.vb`**

[!code-vb[Main](creating-model-classes-with-linq-to-sql-vb/samples/sample4.vb)]

Por fim, a classe MoviesController na listagem 5 usa o padrão Repository. Ele não usa mais classes LINQ to SQL diretamente.

**Listagem 5 – `Controllers\MoviesController.vb`**

[!code-vb[Main](creating-model-classes-with-linq-to-sql-vb/samples/sample5.vb)]

Observe que a classe MoviesController na listagem 5 tem dois construtores. O primeiro construtor, o construtor sem parâmetros, é chamado quando seu aplicativo está em execução. Esse construtor cria uma instância da classe MovieRepository e a passa para o segundo construtor.

O segundo construtor tem um único parâmetro: um parâmetro IMovieRepository. Esse construtor simplesmente atribui o valor do parâmetro a um campo de nível de classe chamado \_Repository.

A classe MoviesController está tirando proveito de um padrão de design de software chamado padrão de injeção de dependência. Em particular, ele está usando algo chamado injeção de dependência de construtor. Você pode ler mais sobre esse padrão lendo o seguinte artigo de Martin Fowler:

[http://martinfowler.com/articles/injection.html](http://martinfowler.com/articles/injection.html)

Observe que todo o código na classe MoviesController (com a exceção do primeiro construtor) interage com a interface IMovieRepository em vez da classe MovieRepository real. O código interage com uma interface abstrata em vez de uma implementação concreta da interface.

Se você quiser modificar a tecnologia de acesso a dados usada pelo aplicativo, poderá simplesmente implementar a interface IMovieRepository com uma classe que usa a tecnologia de acesso ao banco de dado alternativa. Por exemplo, você pode criar uma classe EntityFrameworkMovieRepository ou uma classe SubSonicMovieRepository. Como a classe Controller é programada na interface, você pode passar uma nova implementação de IMovieRepository para a classe Controller e a classe continuaria funcionando.

Além disso, se você quiser testar a classe MoviesController, poderá passar uma classe de repositório de filmes falsa para o MoviesController. Você pode implementar a classe IMovieRepository com uma classe que não acesse o banco de dados, mas que contém todos os métodos necessários da interface IMovieRepository. Dessa forma, você pode testar a classe MoviesController sem realmente acessar um banco de dados real.

## <a name="summary"></a>Resumo

O objetivo deste tutorial foi demonstrar como você pode criar classes de modelo MVC aproveitando o Microsoft LINQ to SQL. Examinamos duas estratégias para a exibição de dados de um aplicativo do ASP.NET MVC. Primeiro, criamos LINQ to SQL classes e usamos as classes diretamente dentro de uma ação do controlador. O uso de classes LINQ to SQL dentro de um controlador permite que você exiba dados de um aplicativo de forma rápida e fácil em aplicativos MVC.

Em seguida, exploramos um caminho um pouco mais difícil, mas, definitivamente, mais virtuoso para exibir dados do banco de dados. Tiramos proveito do padrão de repositório e colocamos toda a lógica de acesso ao banco de dados em uma classe de repositório separada. Em nosso controlador, escrevemos todo o nosso código em uma interface em vez de uma classe concreta. A vantagem do padrão Repository é que ele nos permite alterar facilmente as tecnologias de acesso ao banco de dados no futuro, e ele nos permite testar facilmente nossas classes Controller.

> [!div class="step-by-step"]
> [Anterior](creating-model-classes-with-the-entity-framework-vb.md)
> [Próximo](displaying-a-table-of-database-data-vb.md)
