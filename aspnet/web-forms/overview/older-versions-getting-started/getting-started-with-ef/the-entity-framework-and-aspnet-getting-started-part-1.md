---
uid: web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-1
title: Introdução com Entity Framework 4,0 Database First e ASP.NET 4 Web Forms | Microsoft Docs
author: tdykstra
description: O aplicativo Web de exemplo Contoso University demonstra como criar ASP.NET Web Forms aplicativos usando o Entity Framework 4,0 e o Visual Studio 2010...
ms.author: riande
ms.date: 12/03/2010
ms.assetid: 5cb00916-8f46-491f-be25-4739a615d619
msc.legacyurl: /web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-1
msc.type: authoredcontent
ms.openlocfilehash: fd88b32ad2a65e5b4c7ead15f0d6dc5dc6e97e75
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78630455"
---
# <a name="getting-started-with-entity-framework-40-database-first-and-aspnet-4-web-forms"></a>Introdução com Entity Framework 4,0 Database First e ASP.NET 4 Web Forms

por [Tom Dykstra](https://github.com/tdykstra)

> O aplicativo Web de exemplo Contoso University demonstra como criar ASP.NET Web Forms aplicativos usando o Entity Framework 4,0 e o Visual Studio 2010. O aplicativo de exemplo é um site para uma Universidade fictícia da contoso. Ele inclui funcionalidades como admissão de alunos, criação de cursos e atribuições de instrutor.
> 
> O tutorial mostra exemplos em C#. O [exemplo para download](https://code.msdn.microsoft.com/ASPNET-Web-Forms-97f8ee9a) contém código em ambos C# e Visual Basic.
> 
> ## <a name="database-first"></a>Database First
> 
> Há três maneiras de trabalhar com dados no Entity Framework: *Database First*, *Model First*e *Code First*. Este tutorial é para Database First. Para obter informações sobre as diferenças entre esses fluxos de trabalho e diretrizes sobre como escolher o melhor para seu cenário, consulte [Entity Framework fluxos de trabalho de desenvolvimento](https://msdn.microsoft.com/library/ms178359.aspx#dbfmfcf).
> 
> ## <a name="web-forms"></a>Formulários da Web
> 
> Esta série de tutoriais usa o modelo de Web Forms ASP.NET e pressupõe que você saiba como trabalhar com o ASP.NET Web Forms no Visual Studio. Se não tiver, consulte [introdução com ASP.NET 4,5 Web Forms](../../getting-started/getting-started-with-aspnet-45-web-forms/introduction-and-overview.md). Se você preferir trabalhar com a estrutura MVC do ASP.NET, consulte [introdução com o Entity Framework usando o ASP.NET MVC](../../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).
> 
> ## <a name="software-versions"></a>Versões de software
> 
> | **Mostrado no tutorial** | **Também funciona com** |
> | --- | --- |
> | Windows 7 | Windows 8 |
> | Visual Studio 2010 | Visual Studio 2010 Express para Web. O tutorial não foi testado com versões posteriores do Visual Studio. Há muitas diferenças em seleções de menu, caixas de diálogo e modelos. |
> | .NET 4 | O .NET 4,5 é compatível com o .NET 4, mas o tutorial não foi testado com o .NET 4,5. |
> | Entity Framework 4 | O tutorial não foi testado com versões posteriores do Entity Framework. A partir do Entity Framework 5, o EF usa por padrão o `DbContext API` que foi introduzido com o EF 4,1. O controle de EntityDataSource foi projetado para usar a API `ObjectContext`. Para obter informações sobre como usar o controle EntityDataSource com a API `DbContext`, consulte [esta postagem de blog](https://blogs.msdn.com/b/webdev/archive/2012/09/13/how-to-use-the-entitydatasource-control-with-entity-framework-code-first.aspx). |
> 
> ## <a name="questions"></a>Perguntas
> 
> Se você tiver dúvidas que não estão diretamente relacionadas ao tutorial, poderá lançá-las no fórum de [Entity Framework ASP.net](https://forums.asp.net/1227.aspx), no [Entity Framework e no LINQ to Entities forum](https://social.msdn.microsoft.com/forums/adodotnetentityframework/threads/)ou [stackoverflow.com](http://stackoverflow.com/).

## <a name="overview"></a>Visão geral

O aplicativo que você criará nesses tutoriais é um site de Universidade simples.

[![Image03](the-entity-framework-and-aspnet-getting-started-part-1/_static/image2.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image1.png)

Os usuários podem exibir e atualizar informações de alunos, cursos e instrutores. Algumas das telas que você criará são mostradas abaixo.

[![Image30](the-entity-framework-and-aspnet-getting-started-part-1/_static/image4.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image3.png)

[![Image37](the-entity-framework-and-aspnet-getting-started-part-1/_static/image6.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image5.png)

[![Image31](the-entity-framework-and-aspnet-getting-started-part-1/_static/image8.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image7.png)

[![Image32](the-entity-framework-and-aspnet-getting-started-part-1/_static/image10.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image9.png)

## <a name="creating-the-web-application"></a>Criando o aplicativo Web

Para iniciar o tutorial, abra o Visual Studio e crie um novo projeto de aplicativo Web ASP.NET usando o modelo de **aplicativo web ASP.net** :

[![Image01](the-entity-framework-and-aspnet-getting-started-part-1/_static/image12.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image11.png)

Este modelo cria um projeto de aplicativo Web que já inclui uma folha de estilos e páginas mestras:

[![Image02](the-entity-framework-and-aspnet-getting-started-part-1/_static/image14.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image13.png)

Abra o arquivo *site. Master* e altere "meu aplicativo ASP.net" para "Contoso University".

[!code-html[Main](the-entity-framework-and-aspnet-getting-started-part-1/samples/sample1.html)]

Localize o controle de *menu* chamado `NavigationMenu` e substitua-o pela marcação a seguir, que adiciona itens de menu para as páginas que você criará.

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-1/samples/sample2.aspx)]

Abra a página *Default. aspx* e altere o controle de `Content` chamado `BodyContent` para isso:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-1/samples/sample3.aspx)]

Agora você tem um home page simples com links para as várias páginas que criará:

[![Image03](the-entity-framework-and-aspnet-getting-started-part-1/_static/image16.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image15.png)

## <a name="creating-the-database"></a>Criando o banco de dados

Para esses tutoriais, você usará o Entity Framework Data Model designer para criar automaticamente o modelo de dados com base em um banco de dado existente (geralmente chamado de abordagem do banco de dados *-primeiro* ). Uma alternativa que não é abordada nesta série de tutoriais é criar o modelo de dados manualmente e, em seguida, fazer com que o designer gere scripts que criam o Database (a abordagem do *modelo primeiro* ).

Para o método Database-First usado neste tutorial, a próxima etapa é adicionar um banco de dados ao site. A maneira mais fácil é baixar primeiro o projeto que acompanha este tutorial. Em seguida, clique com o botão direito do mouse na pasta *dados do\_de aplicativos* , selecione **Adicionar item existente**e selecione o arquivo de banco de dado *School. MDF* do projeto baixado.

Uma alternativa é seguir as instruções em [criando o banco de dados de exemplo da escola](https://msdn.microsoft.com/library/bb399731.aspx). Se você baixar o banco de dados ou criá-lo, copie o arquivo *School. MDF* da seguinte pasta para a pasta de *dados de\_* do aplicativo do aplicativo:

`%PROGRAMFILES%\Microsoft SQL Server\MSSQL10.SQLEXPRESS\MSSQL\DATA`

(Esse local do arquivo *. MDF* pressupõe que você esteja usando o SQL Server 2008 Express.)

Se você criar o banco de dados de um script, execute as seguintes etapas para criar um diagrama de banco de dados:

1. Em **Gerenciador de servidores**, expanda **conexões de dados**, expanda *School. MDF*, clique com o botão direito do mouse em **diagramas**e selecione **Adicionar novo diagrama**.

    [![Image35](the-entity-framework-and-aspnet-getting-started-part-1/_static/image18.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image17.png)
2. Selecione todas as tabelas e clique em **Adicionar**.

    [![Image36](the-entity-framework-and-aspnet-getting-started-part-1/_static/image20.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image19.png)

    SQL Server cria um diagrama de banco de dados que mostra tabelas, colunas nas tabelas e relações entre as tabelas. Você pode mover as tabelas para organizá-las, se desejar.
3. Salve o diagrama como "SchoolDiagram" e feche-o.

Se você baixar o arquivo *School. MDF* que acompanha este tutorial, poderá exibir o diagrama de banco de dados clicando duas vezes em **SchoolDiagram** em **diagramas de banco de dados** no **Gerenciador de servidores**.

[![Image38](the-entity-framework-and-aspnet-getting-started-part-1/_static/image22.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image21.png)

O diagrama é semelhante a este (as tabelas podem estar em locais diferentes do que é mostrado aqui):

[![Image04](the-entity-framework-and-aspnet-getting-started-part-1/_static/image24.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image23.png)

## <a name="creating-the-entity-framework-data-model"></a>Criando o modelo de dados Entity Framework

Agora você pode criar um modelo de dados Entity Framework a partir desse banco de dado. Você pode criar o modelo de dados na pasta raiz do aplicativo, mas, para este tutorial, você o colocará em uma pasta chamada *Dal* (para a camada de acesso a dados).

Em **Gerenciador de soluções**, adicione uma pasta de projeto chamada *Dal* (verifique se ela está no projeto, não na solução).

Clique com o botão direito do mouse na pasta *Dal* e selecione **Adicionar** e **novo item**. Em **modelos instalados**, selecione **dados**, selecione o modelo de **modelo de dados de entidade ADO.net** , nomeie-o *SchoolModel. edmx*e clique em **Adicionar**.

[![Image05](the-entity-framework-and-aspnet-getting-started-part-1/_static/image26.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image25.png)

Isso inicia o assistente de Modelo de Dados de Entidade. Na primeira etapa do assistente, a opção **gerar do banco de dados** é selecionada por padrão. Clique em **Próximo**.

[![Image06](the-entity-framework-and-aspnet-getting-started-part-1/_static/image28.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image27.png)

Na etapa **escolher sua conexão de dados** , deixe os valores padrão e clique em **Avançar**. O banco de dados escolar é selecionado por padrão e a configuração de conexão é salva no arquivo *Web. config* como **SchoolEntities**.

[![Image07](the-entity-framework-and-aspnet-getting-started-part-1/_static/image30.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image29.png)

Na etapa **escolher o assistente de objetos de banco de dados** , selecione todas as tabelas, exceto `sysdiagrams` (que foi criado para o diagrama gerado anteriormente) e clique em **concluir**.

[![Image08](the-entity-framework-and-aspnet-getting-started-part-1/_static/image32.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image31.png)

Depois de concluir a criação do modelo, o Visual Studio mostra uma representação gráfica dos objetos de Entity Framework (entidades) que correspondem às suas tabelas de banco de dados. (Assim como no diagrama de banco de dados, o local de elementos individuais pode ser diferente do que você vê nesta ilustração. Você pode arrastar os elementos para corresponder à ilustração, se desejar.)

[![Image09](the-entity-framework-and-aspnet-getting-started-part-1/_static/image34.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image33.png)

## <a name="exploring-the-entity-framework-data-model"></a>Explorando o modelo de dados Entity Framework

Você pode ver que o diagrama de entidade é muito semelhante ao diagrama de banco de dados, com algumas diferenças. Uma diferença é a adição de símbolos no final de cada associação que indicam o tipo de associação (as relações de tabela são chamadas de associações de entidade no modelo de dados):

- Uma associação um-para-zero-ou-um é representada por "1" e "0.. 1".

    [![Image39](the-entity-framework-and-aspnet-getting-started-part-1/_static/image36.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image35.png)

    Nesse caso, uma entidade `Person` pode ou não estar associada a uma entidade `OfficeAssignment`. Uma entidade de `OfficeAssignment` deve ser associada a uma entidade `Person`. Em outras palavras, um instrutor pode ou não ser atribuído a um escritório, e qualquer escritório pode ser atribuído a apenas um instrutor.
- Uma associação um-para-muitos é representada por "1" e "\*".

    [![Image40](the-entity-framework-and-aspnet-getting-started-part-1/_static/image38.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image37.png)

    Nesse caso, uma entidade `Person` pode ou não ter entidades `StudentGrade` associadas. Uma entidade de `StudentGrade` deve ser associada a uma entidade de `Person`. `StudentGrade` entidades realmente representam cursos registrados neste banco de dados; se um aluno estiver registrado em um curso e ainda não houver nenhuma classificação, a propriedade `Grade` será nula. Em outras palavras, um aluno pode não estar inscrito em nenhum curso ainda, pode ser registrado em um único ou pode ser registrado em vários cursos. Cada nível em um curso registrado se aplica a apenas um aluno.
- Uma associação muitos para muitos é representada por "\*" e "\*".

    [![Image41](the-entity-framework-and-aspnet-getting-started-part-1/_static/image40.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image39.png)

    Nesse caso, uma entidade de `Person` pode ou não ter entidades de `Course` associadas e o inverso também é verdadeiro: uma entidade `Course` pode ou não ter entidades `Person` associadas. Em outras palavras, um instrutor pode ensinar vários cursos, e um curso pode ser ensinado por vários instrutores. (Neste banco de dados, essa relação se aplica somente a instrutores; ela não vincula alunos a cursos. Os alunos estão vinculados a cursos pela tabela StudentGrades.)

Outra diferença entre o diagrama e o modelo de dados é a seção de **Propriedades de navegação** adicional para cada entidade. Uma propriedade de navegação de uma entidade referencia entidades relacionadas. Por exemplo, a propriedade `Courses` em uma entidade `Person` contém uma coleção de todas as entidades `Course` relacionadas a essa entidade `Person`.

[![Image12](the-entity-framework-and-aspnet-getting-started-part-1/_static/image42.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image41.png)

Ainda assim, outra diferença entre o banco de dados e o modelo é a ausência da tabela de associação de `CourseInstructor` que é usada para vincular as tabelas `Person` e `Course` em uma relação muitos-para-muitos. As propriedades de navegação permitem que você obtenha `Course` entidades relacionadas da entidade `Person` e entidades de `Person` relacionadas da entidade `Course`, portanto, não é necessário representar a tabela de associação no modelo de dados.

[![Image11](the-entity-framework-and-aspnet-getting-started-part-1/_static/image44.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image43.png)

Para fins deste tutorial, suponha que a coluna `FirstName` da tabela `Person` realmente contenha o nome e o nome do meio de uma pessoa. Você deseja alterar o nome do campo para refletir isso, mas o administrador de banco de dados (DBA) pode não querer alterar o banco de dados. Você pode alterar o nome da propriedade `FirstName` no modelo de dados, deixando seu banco de dado equivalente inalterado.

No designer, clique com o botão direito do mouse em **FirstName** na entidade `Person` e, em seguida, selecione **renomear**.

[![Image13](the-entity-framework-and-aspnet-getting-started-part-1/_static/image46.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image45.png)

Digite o novo nome "FirstMidName". Isso altera a maneira como você se refere à coluna no código sem alterar o banco de dados.

[![Image29](the-entity-framework-and-aspnet-getting-started-part-1/_static/image48.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image47.png)

O navegador de modelos fornece outra maneira de exibir a estrutura do banco de dados, a estrutura do modelo e o mapeamento entre elas. Para vê-lo, clique com o botão direito do mouse em uma área em branco no Entity designer e clique em **navegador de modelos**.

[![Image18](the-entity-framework-and-aspnet-getting-started-part-1/_static/image50.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image49.png)

O painel **navegador de modelos** exibe um modo de exibição de árvore. (O painel **navegador de modelos** pode estar encaixado com o painel de **Gerenciador de soluções** .) O nó **SchoolModel** representa a estrutura do modelo de dados e o nó **SchoolModel. Store** representa a estrutura do banco de dado.

[![Image26](the-entity-framework-and-aspnet-getting-started-part-1/_static/image52.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image51.png)

Expanda **SchoolModel. Store** para ver as tabelas, expanda **tabelas/exibições** para ver tabelas e, em seguida, expanda **curso** para ver as colunas em uma tabela.

[![Image19](the-entity-framework-and-aspnet-getting-started-part-1/_static/image54.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image53.png)

Expanda **SchoolModel**, expanda **tipos de entidade**e, em seguida, expanda o nó do **curso** para ver as entidades e as propriedades dentro das entidades.

[![Image20](the-entity-framework-and-aspnet-getting-started-part-1/_static/image56.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image55.png)

No designer ou no painel **navegador de modelos** , você pode ver como o Entity Framework relaciona os objetos dos dois modelos. Clique com o botão direito do mouse na entidade `Person` e selecione **mapeamento de tabela**.

[![Image21](the-entity-framework-and-aspnet-getting-started-part-1/_static/image58.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image57.png)

Isso abre a janela **detalhes do mapeamento** . Observe que essa janela permite que você veja que a coluna de banco de dados `FirstName` está mapeada para `FirstMidName`, que é o que você renomeou para o modelo de dados.

[![Image22](the-entity-framework-and-aspnet-getting-started-part-1/_static/image60.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image59.png)

O Entity Framework usa XML para armazenar informações sobre o banco de dados, o modelo de dado e os mapeamentos entre eles. O arquivo *SchoolModel. edmx* é, na verdade, um arquivo XML que contém essas informações. O designer renderiza as informações em um formato gráfico, mas você também pode exibir o arquivo como XML clicando com o botão direito do mouse no arquivo *. edmx* em **Gerenciador de soluções**, clicando em **abrir com**e selecionando **editor XML (texto)** . (O designer de modelo de dados e um editor de XML são apenas duas maneiras diferentes de abrir e trabalhar com o mesmo arquivo, de modo que não é possível abrir o designer e abrir o arquivo em um editor de XML ao mesmo tempo.)

Agora você criou um site, um banco de dados e um modelo. Na próxima explicação, você começará a trabalhar com os dados usando o modelo de dados e o controle de `EntityDataSource` ASP.NET.

> [!div class="step-by-step"]
> [Próximo](the-entity-framework-and-aspnet-getting-started-part-2.md)
