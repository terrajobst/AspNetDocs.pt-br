---
uid: mvc/overview/older-versions-1/models-data/displaying-a-table-of-database-data-vb
title: Exibindo uma tabela de dados de banco (VB) | Microsoft Docs
author: microsoft
description: Neste tutorial, demonstrarei dois métodos de exibição de um conjunto de registros de banco de dados. Eu mostro dois métodos de formatação de um conjunto de registros de banco de dados em um ta HTML...
ms.author: riande
ms.date: 10/07/2008
ms.assetid: 5bb4587f-5bcd-44f5-b368-3c1709162b35
msc.legacyurl: /mvc/overview/older-versions-1/models-data/displaying-a-table-of-database-data-vb
msc.type: authoredcontent
ms.openlocfilehash: f2e2489ac8455913f55c746dbe05b9fe8272285b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78543011"
---
# <a name="displaying-a-table-of-database-data-vb"></a>Exibir uma tabela de dados de banco de dados (VB)

pela [Microsoft](https://github.com/microsoft)

[Baixar PDF](https://download.microsoft.com/download/1/1/f/11f721aa-d749-4ed7-bb89-a681b68894e6/ASPNET_MVC_Tutorial_11_VB.pdf)

> Neste tutorial, demonstrarei dois métodos de exibição de um conjunto de registros de banco de dados. Eu mostro dois métodos de formatação de um conjunto de registros de banco de dados em uma tabela HTML. Primeiro, mostro como é possível formatar os registros do banco de dados diretamente em uma exibição. Em seguida, demonstrarei como você pode aproveitar os parciais ao Formatar registros de banco de dados.

O objetivo deste tutorial é explicar como você pode exibir uma tabela HTML de dados de banco de dado em um aplicativo MVC ASP.NET. Primeiro, você aprende a usar as ferramentas do scaffolding incluídas no Visual Studio para gerar uma exibição que exibe um conjunto de registros automaticamente. Em seguida, você aprenderá a usar um parcial como um modelo ao Formatar registros de banco de dados.

## <a name="create-the-model-classes"></a>Criar as classes de modelo

Vamos exibir o conjunto de registros da tabela de banco de dados de filmes. A tabela de banco de dados de filmes contém as seguintes colunas:

<a id="0.4_table01"></a>

| **Nome da Coluna** | **Tipo de Dados** | **Permitir Nulos** |
| --- | --- | --- |
| ID | Int | Falso |
| Title | Nvarchar(200) | Falso |
| Diretor | NVarchar(50) | Falso |
| DateReleased | Datetime | Falso |

Para representar a tabela de filmes em nosso aplicativo MVC ASP.NET, precisamos criar uma classe de modelo. Neste tutorial, usamos o Entity Framework da Microsoft para criar nossas classes de modelo.

> [!NOTE] 
> 
> Neste tutorial, usamos o Entity Framework da Microsoft. No entanto, é importante entender que você pode usar uma variedade de tecnologias diferentes para interagir com um banco de dados de um aplicativo MVC ASP.NET, incluindo LINQ to SQL, NHibernate ou ADO.NET.

Siga estas etapas para iniciar o assistente de Modelo de Dados de Entidade:

1. Clique com o botão direito do mouse na pasta modelos na janela Gerenciador de Soluções e selecione a opção de menu **Adicionar, novo item**.
2. Selecione a categoria de **dados** e selecione o modelo de **modelo de dados de entidade ADO.net** .
3. Dê ao seu modelo de dados o nome *MoviesDBModel. edmx* e clique no botão **Adicionar** .

Depois que você clicar no botão Adicionar, o assistente de Modelo de Dados de Entidade será exibido (consulte a Figura 1). Siga estas etapas para concluir o assistente:

1. Na etapa **escolher conteúdo do modelo** , selecione a opção **gerar do banco de dados** .
2. Na etapa **escolher sua conexão de dados** , use a conexão de dados *MoviesDB. MDF* e o nome *MoviesDBEntities* para as configurações de conexão. Clique no botão **Avançar**.
3. Na etapa **escolher seus objetos de banco de dados** , expanda o nó tabelas, selecione a tabela filmes. Insira os *modelos* de namespace e clique no botão **concluir** .

[![criar classes de LINQ to SQL](displaying-a-table-of-database-data-vb/_static/image1.jpg)](displaying-a-table-of-database-data-vb/_static/image1.png)

**Figura 01**: Criando classes de LINQ to SQL ([clique para exibir a imagem em tamanho normal](displaying-a-table-of-database-data-vb/_static/image2.png))

Depois de concluir o assistente de Modelo de Dados de Entidade, o designer de Modelo de Dados de Entidade é aberto. O designer deve exibir a entidade filmes (veja a Figura 2).

[![o designer de Modelo de Dados de Entidade](displaying-a-table-of-database-data-vb/_static/image2.jpg)](displaying-a-table-of-database-data-vb/_static/image3.png)

**Figura 02**: o Designer de modelo de dados de entidade ([clique para exibir a imagem em tamanho normal](displaying-a-table-of-database-data-vb/_static/image4.png))

Precisamos fazer uma alteração antes de continuarmos. O assistente de dados de entidade gera uma classe de modelo denominada *filmes* que representa a tabela do banco de dados de filmes. Como usaremos a classe Movies para representar um filme específico, precisamos modificar o nome da classe como *filme* , em vez de *filmes* (singular, em vez de plural).

Clique duas vezes no nome da classe na superfície do designer e altere o nome da classe de filmes para filme. Depois de fazer essa alteração, clique no botão **salvar** (o ícone do disquete) para gerar a classe de filme.

## <a name="create-the-movies-controller"></a>Criar o controlador de filmes

Agora que temos uma maneira de representar nossos registros de banco de dados, podemos criar um controlador que retorne a coleção de filmes. Na janela de Gerenciador de Soluções do Visual Studio, clique com o botão direito do mouse na pasta controladores e selecione a opção de menu **Adicionar, controlador** (consulte a Figura 3).

[![o menu Adicionar controlador](displaying-a-table-of-database-data-vb/_static/image3.jpg)](displaying-a-table-of-database-data-vb/_static/image5.png)

**Figura 03**: o menu Adicionar controlador ([clique para exibir a imagem em tamanho normal](displaying-a-table-of-database-data-vb/_static/image6.png))

Quando a caixa de diálogo **Adicionar controlador** for exibida, insira o nome do controlador MovieController (consulte a Figura 4). Clique no botão **Adicionar** para adicionar o novo controlador.

[![caixa de diálogo Adicionar controlador](displaying-a-table-of-database-data-vb/_static/image4.jpg)](displaying-a-table-of-database-data-vb/_static/image7.png)

**Figura 04**: a caixa de diálogo Adicionar controlador ([clique para exibir a imagem em tamanho normal](displaying-a-table-of-database-data-vb/_static/image8.png))

Precisamos modificar a ação de índice () exposta pelo controlador de filme para que ele retorne o conjunto de registros de banco de dados. Modifique o controlador para que fique parecido com o controlador na Listagem 1.

**Listagem 1 – Controllers\MovieController.vb**

[!code-vb[Main](displaying-a-table-of-database-data-vb/samples/sample1.vb)]

Na Listagem 1, a classe MoviesDBEntities é usada para representar o banco de dados MoviesDB. As entidades de expressão *. Movieset. ToList ()* retorna o conjunto de todos os filmes da tabela de banco de dados de filmes.

## <a name="create-the-view"></a>Criar o modo de exibição

A maneira mais fácil de exibir um conjunto de registros de banco de dados em uma tabela HTML é tirar proveito do scaffolding fornecido pelo Visual Studio.

Crie seu aplicativo selecionando a opção de menu **Compilar, Compilar solução**. Você deve compilar seu aplicativo antes de abrir a caixa de diálogo **Adicionar exibição** ou suas classes de dados não aparecerão na caixa de diálogo.

Clique com o botão direito do mouse na ação index () e selecione a opção de menu **Add View** (consulte a Figura 5).

[![adicionar uma exibição](displaying-a-table-of-database-data-vb/_static/image5.jpg)](displaying-a-table-of-database-data-vb/_static/image9.png)

**Figura 05**: adicionando uma exibição ([clique para exibir a imagem em tamanho normal](displaying-a-table-of-database-data-vb/_static/image10.png))

Na caixa de diálogo **Adicionar exibição** , marque a caixa de seleção **criar uma exibição fortemente tipada**. Selecione a classe filme como a **classe de dados de exibição**. Selecione *lista* como o **conteúdo de exibição** (veja a Figura 6). A seleção dessas opções irá gerar uma exibição fortemente tipada que exibe uma lista de filmes.

[![a caixa de diálogo Adicionar exibição](displaying-a-table-of-database-data-vb/_static/image6.jpg)](displaying-a-table-of-database-data-vb/_static/image11.png)

**Figura 06**: a caixa de diálogo Adicionar exibição ([clique para exibir a imagem em tamanho normal](displaying-a-table-of-database-data-vb/_static/image12.png))

Depois que você clicar no botão **Adicionar** , a exibição na Listagem 2 será gerada automaticamente. Essa exibição contém o código necessário para iterar na coleção de filmes e exibir cada uma das propriedades de um filme.

**Listagem 2 – Views\Movie\Index.aspx**

[!code-aspx[Main](displaying-a-table-of-database-data-vb/samples/sample2.aspx)]

Você pode executar o aplicativo selecionando a opção de menu **depurar, iniciar a depuração** (ou pressionar a tecla F5). A execução do aplicativo inicia o Internet Explorer. Se você navegar até a URL do/Movie, verá a página na Figura 7.

[![uma tabela de filmes](displaying-a-table-of-database-data-vb/_static/image7.jpg)](displaying-a-table-of-database-data-vb/_static/image13.png)

**Figura 07**: uma tabela de filmes ([clique para exibir a imagem em tamanho normal](displaying-a-table-of-database-data-vb/_static/image14.png))

Se você não gostar de nada sobre a aparência da grade de registros do banco de dados na Figura 7, poderá simplesmente modificar a exibição do índice. Por exemplo, você pode alterar o cabeçalho *DateReleased* para *data de lançamento* modificando a exibição de índice.

## <a name="create-a-template-with-a-partial"></a>Criar um modelo com um parcial

Quando uma exibição é muito complicada, é uma boa ideia começar a dividir a exibição em partes parciais. O uso de parciais torna suas exibições mais fáceis de entender e manter. Vamos criar um parcial que podemos usar como modelo para formatar cada um dos registros do banco de dados de filmes.

Siga estas etapas para criar o parcial:

1. Clique com o botão direito do mouse na pasta Views\Movie e selecione a opção de menu **Adicionar modo de exibição**.
2. Marque a caixa de seleção rotulada *criar uma exibição parcial (. ascx)* .
3. Nomeie o *movietemplate*parcial.
4. Marque a caixa de seleção rotulada **criar uma exibição fortemente tipada**.
5. Selecione filme como a *classe de dados de exibição*.
6. Selecione vazio como o *conteúdo de exibição*.
7. Clique no botão **Adicionar** para adicionar o parcial ao seu projeto.

Depois de concluir essas etapas, modifique o Movietemplate parcial para se parecer com a listagem 3.

**Listagem 3 – Views\Movie\MovieTemplate.ascx**

[!code-aspx[Main](displaying-a-table-of-database-data-vb/samples/sample3.aspx)]

O parcial na Listagem 3 contém um modelo para uma única linha de registros.

A exibição de índice modificada na Listagem 4 usa o Movietemplate parcial.

**Listagem 4 – Views\Movie\Index.aspx**

[!code-aspx[Main](displaying-a-table-of-database-data-vb/samples/sample4.aspx)]

A exibição na Listagem 4 contém um loop for each que itera em todos os filmes. Para cada filme, o Movietemplate parcial é usado para formatar o filme. O Movietemplate é renderizado chamando o método auxiliar RenderPartial ().

A exibição de índice modificado renderiza a mesma tabela HTML de registros de banco de dados. No entanto, a exibição foi bastante simplificada.

O método RenderPartial () é diferente da maioria dos outros métodos auxiliares porque ele não retorna uma cadeia de caracteres. Portanto, você deve chamar o método RenderPartial () usando &lt;% HTML. RenderPartial ()%&gt; em vez de &lt;% = HTML. RenderPartial ()%&gt;.

## <a name="summary"></a>Resumo

O objetivo deste tutorial foi explicar como você pode exibir um conjunto de registros de banco de dados em uma tabela HTML. Primeiro, você aprendeu como retornar um conjunto de registros de banco de dados de uma ação de controlador aproveitando o Entity Framework da Microsoft. Em seguida, você aprendeu a usar o Visual Studio scaffolding para gerar uma exibição que exibe uma coleção de itens automaticamente. Por fim, você aprendeu a simplificar a exibição tirando proveito de um parcial. Você aprendeu a usar um parcial como um modelo para que possa Formatar cada registro de banco de dados.

> [!div class="step-by-step"]
> [Anterior](creating-model-classes-with-linq-to-sql-vb.md)
> [Próximo](performing-simple-validation-vb.md)
