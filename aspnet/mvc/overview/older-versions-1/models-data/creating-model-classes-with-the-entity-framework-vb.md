---
uid: mvc/overview/older-versions-1/models-data/creating-model-classes-with-the-entity-framework-vb
title: Criando classes de modelo com o Entity Framework (VB) | Microsoft Docs
author: microsoft
description: Neste tutorial, você aprenderá a usar o ASP.NET MVC com o Microsoft Entity Framework. Você aprende a usar o assistente de entidade para criar uma entidade ADO.NET da...
ms.author: riande
ms.date: 01/27/2009
ms.assetid: ff8322c9-12f3-4e24-aba6-a38046b9bb0d
msc.legacyurl: /mvc/overview/older-versions-1/models-data/creating-model-classes-with-the-entity-framework-vb
msc.type: authoredcontent
ms.openlocfilehash: f6c896c6f5f6d898ac6f99d5998fb29cb73bcb10
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78543326"
---
# <a name="creating-model-classes-with-the-entity-framework-vb"></a>Criação de classes de modelo com o Entity Framework (VB)

pela [Microsoft](https://github.com/microsoft)

> Neste tutorial, você aprenderá a usar o ASP.NET MVC com o Microsoft Entity Framework. Você aprende a usar o assistente de entidade para criar um Modelo de Dados de Entidade ADO.NET. No decorrer deste tutorial, criamos um aplicativo Web que ilustra como selecionar, inserir, atualizar e excluir dados de banco usando o Entity Framework.

O objetivo deste tutorial é explicar como você pode criar classes de acesso a dados usando o Microsoft Entity Framework ao criar um aplicativo MVC ASP.NET. Este tutorial não pressupõe nenhum conhecimento anterior do Microsoft Entity Framework. Ao final deste tutorial, você entenderá como usar o Entity Framework para selecionar, inserir, atualizar e excluir registros de banco de dados.

O Microsoft Entity Framework é uma ferramenta de mapeamento relacional de objeto (O/RM) que permite que você gere automaticamente uma camada de acesso a dados de um banco de dado. A Entity Framework permite evitar o trabalho entediante de criar classes de acesso a dados manualmente.

> [!NOTE] 
> 
> Não há nenhuma conexão essencial entre o ASP.NET MVC e o Microsoft Entity Framework. Há várias alternativas para o Entity Framework que você pode usar com o ASP.NET MVC. Por exemplo, você pode criar suas classes de modelo MVC usando outras ferramentas O/RM, como Microsoft LINQ to SQL, NHibernate ou SubSonic.

Para ilustrar como você pode usar o Microsoft Entity Framework com o ASP.NET MVC, criaremos um aplicativo de exemplo simples. Criaremos um aplicativo de banco de dados de filme que permite exibir e editar registros de banco de dados de filmes.

Este tutorial pressupõe que você tenha o Visual Studio 2008 ou o Visual Web Developer 2008 com Service Pack 1. Você precisa do Service Pack 1 para usar o Entity Framework. Você pode baixar o Visual Studio 2008 Service Pack 1 ou o Visual Web Developer com Service Pack 1 do seguinte endereço:

> [https://www.asp.net/downloads/](https://www.asp.net/downloads)

## <a name="creating-the-movie-sample-database"></a>Criando o banco de dados de exemplo de filme

O aplicativo de banco de dados de filmes usa uma tabela de banco de dados chamada filmes que contém as seguintes colunas:

| Nome da coluna | Tipo de Dados | Permitir nulos? | É chave primária? |
| --- | --- | --- | --- |
| ID | INT | Falso | True |
| Title | nvarchar(100) | Falso | Falso |
| Diretor | nvarchar(100) | Falso | Falso |

Você pode adicionar essa tabela a um projeto MVC do ASP.NET seguindo estas etapas:

1. Clique com o botão direito do mouse na pasta\_dados do aplicativo na janela Gerenciador de Soluções e selecione a opção de menu **Adicionar, novo item.**
2. Na caixa de diálogo **Adicionar novo item** , selecione **SQL Server banco de dados**, dê ao banco de dados o nome MoviesDB. MDF e clique no botão **Adicionar** .
3. Clique duas vezes no arquivo MoviesDB. MDF para abrir a janela Gerenciador de Servidores/Gerenciador de Banco de Dados.
4. Expanda a conexão de banco de dados MoviesDB. MDF, clique com o botão direito do mouse na pasta tabelas e selecione a opção de menu **Adicionar nova tabela**.
5. No Designer de Tabela, adicione as colunas ID, título e diretor.
6. Clique no botão **salvar** (ele tem o ícone do disquete) para salvar a nova tabela com o nome filmes.

Depois de criar a tabela de banco de dados de filmes, você deverá adicionar alguns exemplos à tabela. Clique com o botão direito do mouse na tabela filmes e selecione a opção de menu **Mostrar dados da tabela**. Você pode inserir dados de filme falsos na grade exibida.

## <a name="creating-the-adonet-entity-data-model"></a>Criando o Modelo de Dados de Entidade ADO.NET

Para usar o Entity Framework, você precisa criar um Modelo de Dados de Entidade. Você pode aproveitar o *Assistente de modelo de dados de entidade* do Visual Studio para gerar um modelo de dados de entidade de um banco de dados automaticamente.

Siga estas etapas:

1. Clique com o botão direito do mouse na pasta modelos na janela Gerenciador de Soluções e selecione a opção de menu **Adicionar, novo item**.
2. Na caixa de diálogo **Adicionar novo item** , selecione a categoria de dados (veja a Figura 1).
3. Selecione o modelo de **Modelo de Dados de Entidade ADO.net** , dê ao modelo de dados de entidade o nome MoviesDBModel. edmx e clique no botão **Adicionar** . Clicar no botão **Adicionar** inicia o assistente de modelo de dados.
4. Na etapa **escolher conteúdo do modelo** , escolha a opção **gerar de um banco de dados** e clique no botão **Avançar** (consulte a Figura 2).
5. Na etapa **escolher sua conexão de dados** , selecione a conexão de banco de MoviesDB. MDF, insira o nome configurações de conexão de entidades MoviesDBEntities e clique no botão **Avançar** (consulte a Figura 3).
6. Na etapa **escolher seus objetos de banco de dados** , selecione a tabela de banco de dados de filme e clique no botão **concluir** (consulte a Figura 4).

Depois de concluir essas etapas, o Entity Designer (ADO.NET Modelo de Dados de Entidade designer) é aberto.

**Figura 1 – Criando um novo Modelo de Dados de Entidade**

![clip_image002](creating-model-classes-with-the-entity-framework-vb/_static/image1.jpg)

**Figura 2 – escolher a etapa de conteúdo do modelo**

![clip_image004](creating-model-classes-with-the-entity-framework-vb/_static/image2.jpg)

**Figura 3 – escolher sua conexão de dados**

![clip_image006](creating-model-classes-with-the-entity-framework-vb/_static/image3.jpg)

**Figura 4 – escolher seus objetos de banco de dados**

![clip_image008](creating-model-classes-with-the-entity-framework-vb/_static/image4.jpg)

## <a name="modifying-the-adonet-entity-data-model"></a>Modificando o Modelo de Dados de Entidade ADO.NET

Depois de criar um Modelo de Dados de Entidade, você pode modificar o modelo aproveitando o Entity Designer (consulte a Figura 5). Você pode abrir o Entity Designer a qualquer momento clicando duas vezes no arquivo MoviesDBModel. edmx contido na pasta modelos dentro da janela Gerenciador de Soluções.

**Figura 5 – o designer de Modelo de Dados de Entidade ADO.NET**

![clip_image010](creating-model-classes-with-the-entity-framework-vb/_static/image5.jpg)

Por exemplo, você pode usar a Entity Designer para alterar os nomes das classes que o assistente de dados de modelo de entidade gera. O assistente criou uma nova classe de acesso a dados chamada filmes. Em outras palavras, o assistente deu à classe o mesmo nome da tabela de banco de dados. Como usaremos essa classe para representar uma determinada instância de filme, devemos renomear a classe de filmes para filme.

Se você quiser renomear uma classe de entidade, poderá clicar duas vezes no nome da classe na Entity Designer e inserir um novo nome (consulte a Figura 6). Como alternativa, você pode alterar o nome de uma entidade no janela Propriedades depois de selecionar uma entidade na Entity Designer.

**Figura 6 – alterando um nome de entidade**

![clip_image012](creating-model-classes-with-the-entity-framework-vb/_static/image6.jpg)

Lembre-se de salvar o Modelo de Dados de Entidade depois de fazer uma modificação clicando no botão salvar (o ícone do disquete). Nos bastidores, o Entity Designer gera um conjunto de classes Visual Basic .NET. Você pode exibir essas classes abrindo o arquivo MoviesDBModel. designer. vb na janela Gerenciador de Soluções.

Não modifique o código no arquivo designer. vb, pois suas alterações serão substituídas na próxima vez que você usar o Entity Designer. Se você quiser estender a funcionalidade das classes de entidade definidas no arquivo designer. vb, poderá criar *classes parciais* em arquivos separados.

#### <a name="selecting-database-records-with-the-entity-framework"></a>Selecionando registros de banco de dados com o Entity Framework

Vamos começar a criar nosso aplicativo de banco de dados de filmes criando uma página que exibe uma lista de registros de filmes. O controlador Home na Listagem 1 expõe uma ação chamada index (). A ação index () retorna todos os registros de filme da tabela do banco de dados de filmes aproveitando o Entity Framework.

**Listagem 1 – Controllers\HomeController.vb**

[!code-vb[Main](creating-model-classes-with-the-entity-framework-vb/samples/sample1.vb)]

Observe que o controlador na Listagem 1 inclui um construtor. O construtor inicializa um campo de nível de classe chamado \_DB. O campo \_DB representa as entidades de banco de dados geradas pelo Microsoft Entity Framework. O campo \_DB é uma instância da classe MoviesDBEntities que foi gerada pelo Entity Designer.

O campo \_DB é usado dentro da ação index () para recuperar os registros da tabela de banco de dados Movies. A expressão \_DB. O movieset representa todos os registros da tabela de banco de dados de filmes. O método ToList () é usado para converter o conjunto de filmes em uma coleção genérica de objetos de filme: List (Of Movie).

Os registros de filme são recuperados com a ajuda de LINQ to Entities. A ação index () na Listagem 1 usa a *sintaxe do método* LINQ para recuperar o conjunto de registros do banco de dados. Se preferir, você pode usar a *sintaxe de consulta* LINQ em vez disso. As duas instruções a seguir fazem a mesma coisa:

[!code-vb[Main](creating-model-classes-with-the-entity-framework-vb/samples/sample2.vb)]

Use qualquer sintaxe LINQ – sintaxe de método ou sintaxe de consulta – que você acha mais intuitivo. Não há nenhuma diferença no desempenho entre as duas abordagens – a única diferença é o estilo.

A exibição na Listagem 2 é usada para exibir os registros do filme.

**Listagem 2 – Views\Home\Index.aspx**

[!code-aspx[Main](creating-model-classes-with-the-entity-framework-vb/samples/sample3.aspx)]

A exibição na Listagem 2 contém um loop **for each** que itera em cada registro de filme e exibe os valores das propriedades Title e director do registro de filme. Observe que um link editar e excluir é exibido ao lado de cada registro. Além disso, um link Adicionar filme é exibido na parte inferior da exibição (veja a Figura 7).

**Figura 7 – a exibição do índice**

![clip_image014](creating-model-classes-with-the-entity-framework-vb/_static/image7.jpg)

A exibição de índice é uma *exibição tipada*. A exibição de índice tem uma diretiva &lt;% @ Page%&gt; que inclui um atributo Inherits. O atributo Inherits converte a propriedade ViewData. Model em uma coleção de lista genérica fortemente tipada de objetos de filme – uma lista (de filme).

## <a name="inserting-database-records-with-the-entity-framework"></a>Inserindo registros de banco de dados com o Entity Framework

Você pode usar a Entity Framework para facilitar a inserção de novos registros em uma tabela de banco de dados. A listagem 3 contém duas novas ações adicionadas à classe Home Controller que você pode usar para inserir novos registros na tabela de banco de dados de filmes.

**Listagem 3 – Controllers\HomeController.vb (adicionar métodos)**

[!code-vb[Main](creating-model-classes-with-the-entity-framework-vb/samples/sample4.vb)]

A primeira ação Add () simplesmente retorna uma exibição. A exibição contém um formulário para adicionar um novo registro de banco de dados de filme (consulte a Figura 8). Quando você envia o formulário, a segunda ação Add () é invocada.

Observe que a segunda ação Add () é decorada com o atributo AcceptVerbs. Essa ação pode ser invocada somente ao executar uma operação HTTP POST. Em outras palavras, essa ação só pode ser chamada durante a postagem de um formulário HTML.

A segunda ação Add () cria uma nova instância da classe Entity Framework Movie com a ajuda do método TryUpdateModel () do ASP.NET MVC. O método TryUpdateModel () usa os campos do Formuláriocollection passados para o método Add () e atribui os valores desses campos de formulário HTML à classe Movie.

Ao usar o Entity Framework, você deve fornecer uma "lista branca" de propriedades ao usar os métodos TryUpdateModel ou UpdateModel para atualizar as propriedades de uma classe de entidade.

Em seguida, a ação Add () executa uma validação de formulário simples. A ação verifica se as propriedades Title e director têm valores. Se houver um erro de validação, uma mensagem de erro de validação será adicionada a ModelState.

Se não houver nenhum erro de validação, um novo registro de filme será adicionado à tabela de banco de dados de filmes com a ajuda do Entity Framework. O novo registro é adicionado ao banco de dados com as duas linhas de código a seguir:

[!code-vb[Main](creating-model-classes-with-the-entity-framework-vb/samples/sample5.vb)]

A primeira linha de código adiciona a nova entidade de filme ao conjunto de filmes que está sendo acompanhado pelo Entity Framework. A segunda linha de código salva quaisquer alterações feitas nos filmes que estão sendo controlados de volta para o banco de dados subjacente.

**Figura 8 – a exibição adicionar**

![clip_image016](creating-model-classes-with-the-entity-framework-vb/_static/image8.jpg)

## <a name="updating-database-records-with-the-entity-framework"></a>Atualizando registros de banco de dados com o Entity Framework

Você pode seguir quase a mesma abordagem para editar um registro de banco de dados com o Entity Framework como a abordagem que acabamos de seguir para inserir um novo registro de banco de dados. A listagem 4 contém duas novas ações do controlador nomeadas editar (). A primeira ação Edit () retorna um formulário HTML para editar um registro de filme. A segunda ação editar () tenta atualizar o banco de dados.

**Listagem 4 – Controllers\HomeController.vb (métodos de edição)**

[!code-vb[Main](creating-model-classes-with-the-entity-framework-vb/samples/sample6.vb)]

A segunda ação Edit () começa recuperando o registro do filme do banco de dados que corresponde à ID do filme que está sendo editado. A instrução LINQ to Entities a seguir captura o primeiro registro de banco de dados que corresponde a uma ID específica:

[!code-vb[Main](creating-model-classes-with-the-entity-framework-vb/samples/sample7.vb)]

Em seguida, o método TryUpdateModel () é usado para atribuir os valores dos campos de formulário HTML às propriedades da entidade de filme. Observe que uma lista branca é fornecida para especificar as propriedades exatas a serem atualizadas.

Em seguida, algumas validações simples são executadas para verificar se o título do filme e as propriedades do diretor têm valores. Se uma das propriedades não tiver um valor, uma mensagem de erro de validação será adicionada a ModelState e ModelState. IsValid retornará o valor false.

Por fim, se não houver nenhum erro de validação, a tabela de banco de dados de filmes subjacentes será atualizada com as alterações chamando o método SaveChanges ().

Ao editar registros de banco de dados, você precisa passar a ID do registro que está sendo editado para a ação do controlador que executa a atualização do banco de dados. Caso contrário, a ação do controlador não saberá qual registro será atualizado no banco de dados subjacente. O modo de exibição de edição, contido na listagem 5, inclui um campo de formulário oculto que representa a ID do registro de banco de dados que está sendo editado.

**Listagem 5 – Views\Home\Edit.aspx**

[!code-aspx[Main](creating-model-classes-with-the-entity-framework-vb/samples/sample8.aspx)]

## <a name="deleting-database-records-with-the-entity-framework"></a>Excluindo registros de banco de dados com o Entity Framework

A operação final do banco de dados, que precisamos resolver neste tutorial, é excluir registros de banco de dados. Você pode usar a ação do controlador na Listagem 6 para excluir um registro de banco de dados específico.

**Listagem 6--\Controllers\HomeController.vb (ação de exclusão)**

[!code-vb[Main](creating-model-classes-with-the-entity-framework-vb/samples/sample9.vb)]

A ação DELETE () primeiro recupera a entidade de filme que corresponde à ID passada para a ação. Em seguida, o filme é excluído do banco de dados chamando o método ExcluirObjeto () seguido pelo método SaveChanges (). Por fim, o usuário é Redirecionado de volta para a exibição de índice.

## <a name="summary"></a>Resumo

A finalidade deste tutorial foi demonstrar como você pode criar aplicativos Web controlados por banco de dados aproveitando o ASP.NET MVC e o Microsoft Entity Framework. Você aprendeu a criar um aplicativo que permite que você selecione, insira, atualize e exclua registros de banco de dados.

Primeiro, discutimos como você pode usar o assistente de Modelo de Dados de Entidade para gerar um Modelo de Dados de Entidade de dentro do Visual Studio. Em seguida, você aprenderá a usar LINQ to Entities para recuperar um conjunto de registros de banco de dados de uma tabela de banco de dados. Por fim, usamos o Entity Framework para inserir, atualizar e excluir registros de banco de dados.

> [!div class="step-by-step"]
> [Anterior](validation-with-the-data-annotation-validators-cs.md)
> [Próximo](creating-model-classes-with-linq-to-sql-vb.md)
