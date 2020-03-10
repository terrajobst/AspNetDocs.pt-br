---
uid: web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-3
title: Introdução com Entity Framework 4,0 Database First e ASP.NET 4 Web Forms-parte 3 | Microsoft Docs
author: tdykstra
description: O aplicativo Web de exemplo Contoso University demonstra como criar ASP.NET Web Forms aplicativos usando o Entity Framework. O aplicativo de exemplo é...
ms.author: riande
ms.date: 12/03/2010
ms.assetid: ccdc3f8c-2568-40a7-8f8b-3c23d2e05388
msc.legacyurl: /web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-3
msc.type: authoredcontent
ms.openlocfilehash: 88debb11a9157dce9ff000b1cb459b876dbceaf3
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78643244"
---
# <a name="getting-started-with-entity-framework-40-database-first-and-aspnet-4-web-forms---part-3"></a>Introdução com Entity Framework 4,0 Database First e ASP.NET 4 Web Forms-parte 3

por [Tom Dykstra](https://github.com/tdykstra)

> O aplicativo Web de exemplo Contoso University demonstra como criar ASP.NET Web Forms aplicativos usando o Entity Framework 4,0 e o Visual Studio 2010. Para obter informações sobre a série de tutoriais, consulte [o primeiro tutorial da série](the-entity-framework-and-aspnet-getting-started-part-1.md)

## <a name="filtering-ordering-and-grouping-data"></a>Filtragem, ordenação e agrupamento de dados

No tutorial anterior, você usou o controle de `EntityDataSource` para exibir e editar dados. Neste tutorial, você filtrará, ordenará e agrupará os dados. Quando você faz isso definindo as propriedades do controle de `EntityDataSource`, a sintaxe é diferente de outros controles de fonte de dados. Como você verá, no entanto, você pode usar o controle de `QueryExtender` para minimizar essas diferenças.

Você alterará a página *students. aspx* para filtrar os alunos, classificar por nome e pesquisar o nome. Você também alterará a página *courses. aspx* para exibir os cursos do departamento selecionado e procurar cursos por nome. Por fim, você adicionará estatísticas de estudante à página *about. aspx* .

[![Image02](the-entity-framework-and-aspnet-getting-started-part-3/_static/image2.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image1.png)

[![Image11](the-entity-framework-and-aspnet-getting-started-part-3/_static/image4.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image3.png)

[![Image10](the-entity-framework-and-aspnet-getting-started-part-3/_static/image6.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image5.png)

[![Image14](the-entity-framework-and-aspnet-getting-started-part-3/_static/image8.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image7.png)

## <a name="using-the-entitydatasource-where-property-to-filter-data"></a>Usando a Propriedade EntityDataSource "Where" para filtrar dados

Abra a página *students. aspx* que você criou no tutorial anterior. Como configurado atualmente, o controle de `GridView` na página exibe todos os nomes do conjunto de entidades de `People`. No entanto, você deseja mostrar somente os alunos, que podem ser encontrados selecionando `Person` entidades que têm datas de registro não nulas.

Alterne para o modo de exibição de **design** e selecione o controle de `EntityDataSource`. Na janela **Propriedades** , defina a propriedade `Where` como `it.EnrollmentDate is not null`.

[![Image01](the-entity-framework-and-aspnet-getting-started-part-3/_static/image10.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image9.png)

A sintaxe usada na propriedade `Where` do controle de `EntityDataSource` é Entity SQL. O Entity SQL é semelhante ao Transact-SQL, mas é personalizado para uso com entidades em vez de objetos de banco de dados. No `it.EnrollmentDate is not null`de expressão, a palavra `it` representa uma referência à entidade retornada pela consulta. Portanto, `it.EnrollmentDate` se refere à propriedade `EnrollmentDate` da entidade `Person` que o controle de `EntityDataSource` retorna.

Execute a página. A lista de alunos agora contém apenas alunos. (Não há linhas exibidas em que não haja nenhuma data de registro.)

[![Image02](the-entity-framework-and-aspnet-getting-started-part-3/_static/image12.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image11.png)

## <a name="using-the-entitydatasource-orderby-property-to-order-data"></a>Usando a propriedade de EntityDataSource "OrderBy" para ordenar dados

Você também deseja que essa lista esteja em ordem de nome quando for exibida pela primeira vez. Com a página *students. aspx* ainda aberta no modo **design** e com o controle de `EntityDataSource` ainda selecionado, na janela **Propriedades** , defina a propriedade **OrderBy** como `it.LastName`.

[![Image05](the-entity-framework-and-aspnet-getting-started-part-3/_static/image14.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image13.png)

Execute a página. A lista de alunos agora está em ordem por sobrenome.

[![Image04](the-entity-framework-and-aspnet-getting-started-part-3/_static/image16.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image15.png)

## <a name="using-a-control-parameter-to-set-the-where-property"></a>Usando um parâmetro de controle para definir a propriedade "Where"

Assim como ocorre com outros controles de fonte de dados, você pode passar valores de parâmetro para a propriedade `Where`. Na página *cursos. aspx* que você criou na parte 2 do tutorial, você pode usar esse método para exibir os cursos associados ao departamento que um usuário seleciona na lista suspensa.

Abra o *courses. aspx* e alterne para o modo de exibição de **design** . Adicione um segundo controle `EntityDataSource` à página e nomeie-o `CoursesEntityDataSource`. Conecte-o ao modelo de `SchoolEntities` e selecione `Courses` como o valor **EntitySetName** .

Na janela **Propriedades** , clique nas reticências na caixa de propriedade **Where** . (Verifique se o controle de `CoursesEntityDataSource` ainda está selecionado antes de usar a janela **Propriedades** .)

[![Image06](the-entity-framework-and-aspnet-getting-started-part-3/_static/image18.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image17.png)

A caixa de diálogo **Editor de expressão** é exibida. Nessa caixa de diálogo, selecione **gerar automaticamente a expressão WHERE com base nos parâmetros fornecidos**e clique em **Adicionar parâmetro**. Nomeie o parâmetro `DepartmentID`, selecione **Control** como o valor de **origem do parâmetro** e selecione **DepartmentsDropDownList** como o valor de **ControlID** .

[![Image07](the-entity-framework-and-aspnet-getting-started-part-3/_static/image20.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image19.png)

Clique em **mostrar propriedades avançadas**e, na janela **Propriedades** da caixa de diálogo **Editor de expressão** , altere a propriedade `Type` para `Int32`.

[![Image15](the-entity-framework-and-aspnet-getting-started-part-3/_static/image22.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image21.png)

Quando terminar, clique em **OK**.

Abaixo da lista suspensa, adicione um controle de `GridView` à página e nomeie-o `CoursesGridView`. Conecte-o ao controle da fonte de dados `CoursesEntityDataSource`, clique em **Atualizar esquema**, clique em **Editar colunas**e remova a coluna `DepartmentID`. A marcação de controle de `GridView` é semelhante ao exemplo a seguir.

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-3/samples/sample1.aspx)]

Quando o usuário altera o departamento selecionado na lista suspensa, você deseja que a lista de cursos associados seja alterada automaticamente. Para fazer isso acontecer, selecione a lista suspensa e, na janela **Propriedades** , defina a propriedade `AutoPostBack` como `True`.

[![Image08](the-entity-framework-and-aspnet-getting-started-part-3/_static/image24.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image23.png)

Agora que você terminou de usar o designer, alterne para o modo de exibição de **origem** e substitua as propriedades de `ConnectionString` e de nome de `DefaultContainer` do controle de `CoursesEntityDataSource` pelo atributo `ContextTypeName="ContosoUniversity.DAL.SchoolEntities"`. Quando você terminar, a marcação para o controle será semelhante ao exemplo a seguir.

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-3/samples/sample2.aspx)]

Execute a página e use a lista suspensa para selecionar diferentes departamentos. Somente os cursos que são oferecidos pelo departamento selecionado são exibidos no controle de `GridView`.

[![Image09](the-entity-framework-and-aspnet-getting-started-part-3/_static/image26.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image25.png)

## <a name="using-the-entitydatasource-groupby-property-to-group-data"></a>Usando a propriedade de EntityDataSource "GroupBy" para agrupar dados

Suponha que a Contoso University queira colocar algumas estatísticas de corpo de aluno em sua página About. Especificamente, ele deseja mostrar uma divisão de números de alunos pela data em que eles foram registrados.

Abra *about. aspx*e, na exibição de **código-fonte** , substitua o conteúdo existente do controle de `BodyContent` por "estatísticas de corpo do aluno" entre marcas de `h2`:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-3/samples/sample3.aspx)]

Depois do título, adicione um controle de `EntityDataSource` e nomeie-o `StudentStatisticsEntityDataSource`. Conecte-o à `SchoolEntities`, selecione o conjunto de entidades de `People` e deixe a caixa de **seleção** no assistente inalterada. Defina as seguintes propriedades na janela **Propriedades** :

- Para filtrar somente para estudantes, defina a propriedade `Where` como `it.EnrollmentDate is not null`.
- Para agrupar os resultados pela data de registro, defina a propriedade `GroupBy` como `it.EnrollmentDate`.
- Para selecionar a data de registro e o número de alunos, defina a propriedade `Select` como `it.EnrollmentDate, Count(it.EnrollmentDate) AS NumberOfStudents`.
- Para ordenar os resultados pela data de registro, defina a propriedade `OrderBy` como `it.EnrollmentDate`.

No modo de exibição de **origem** , substitua as propriedades de nome `ConnectionString` e `DefaultContainer` por uma propriedade `ContextTypeName`. A marcação de controle de `EntityDataSource` agora é semelhante ao exemplo a seguir.

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-3/samples/sample4.aspx)]

A sintaxe das propriedades `Select`, `GroupBy`e `Where` é semelhante a Transact-SQL, exceto para a palavra-chave `it` que especifica a entidade atual.

Adicione a marcação a seguir para criar um controle de `GridView` para exibir os dados.

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-3/samples/sample5.aspx)]

Execute a página para ver uma lista que mostra o número de alunos por data de registro.

[![Image10](the-entity-framework-and-aspnet-getting-started-part-3/_static/image28.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image27.png)

## <a name="using-the-queryextender-control-for-filtering-and-ordering"></a>Usando o controle QueryExtender para filtragem e ordenação

O controle `QueryExtender` fornece uma maneira de especificar filtragem e classificação na marcação. A sintaxe é independente do DBMS (sistema de gerenciamento de banco de dados) que você está usando. Também é geralmente independente do Entity Framework, com a exceção de que a sintaxe usada para propriedades de navegação é exclusiva para o Entity Framework.

Nesta parte do tutorial, você usará um controle `QueryExtender` para filtrar e ordenar dados, e um dos campos order-by será uma propriedade de navegação.

(Se você preferir usar código em vez de marcação para estender as consultas que são geradas automaticamente pelo controle de `EntityDataSource`, você pode fazer isso manipulando o evento `QueryCreated`. É assim que o controle de `QueryExtender` estende `EntityDataSource` consultas de controle também.)

Abra a página *cursos. aspx* e, abaixo da marcação que você adicionou anteriormente, insira a marcação a seguir para criar um título, uma caixa de texto para inserir cadeias de caracteres de pesquisa, um botão de pesquisa e um `EntityDataSource` controle associado ao conjunto de entidades de `Courses`.

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-3/samples/sample6.aspx)]

Observe que a propriedade `Include` do controle de `EntityDataSource` está definida como `Department`. No banco de dados, a tabela `Course` não contém o nome do departamento; Ele contém um `DepartmentID` coluna de chave estrangeira. Se você estava consultando o banco de dados diretamente, para obter o nome do departamento junto com os dado do curso, seria necessário unir as tabelas `Course` e `Department`. Ao definir a propriedade `Include` como `Department`, você especifica que a Entity Framework deve fazer o trabalho de obter a entidade de `Department` relacionada quando ela obtém uma entidade de `Course`. Em seguida, a entidade `Department` é armazenada na propriedade de navegação `Department` da entidade `Course`. (Por padrão, a classe `SchoolEntities` que foi gerada pelo designer de modelo de dados recupera dados relacionados quando necessário, e você o controle da fonte de dados para essa classe, de modo que a definição da propriedade `Include` não é necessária. No entanto, configurá-lo melhora o desempenho da página, pois, caso contrário, o Entity Framework faria chamadas separadas para o banco de dados a fim de recuperá-los para as entidades de `Course` e para as entidades de `Department` relacionadas.)

Após o controle de `EntityDataSource` que você acabou de criar, insira a marcação a seguir para criar um controle de `QueryExtender` associado a esse controle de `EntityDataSource`.

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-3/samples/sample7.aspx)]

O elemento `SearchExpression` especifica que você deseja selecionar os cursos cujos títulos correspondem ao valor inserido na caixa de texto. Somente quantos caracteres forem inseridos na caixa de texto serão comparados, pois a propriedade `SearchType` especifica `StartsWith`.

O elemento `OrderByExpression` especifica que o conjunto de resultados será ordenado pelo título do curso dentro do nome do departamento. Observe como o nome do departamento é especificado: `Department.Name`. Como a associação entre a entidade de `Course` e a entidade `Department` é um-para-um, a propriedade de navegação `Department` contém uma entidade `Department`. (Se essa fosse uma relação um-para-muitos, a propriedade conteria uma coleção.) Para obter o nome do departamento, você deve especificar a propriedade `Name` da entidade `Department`.

Por fim, adicione um controle de `GridView` para exibir a lista de cursos:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-3/samples/sample8.aspx)]

A primeira coluna é um campo de modelo que exibe o nome do departamento. A expressão DataBinding especifica `Department.Name`, assim como você viu no controle `QueryExtender`.

Execute a página. A exibição inicial mostra uma lista de todos os cursos em ordem por departamento e, em seguida, por título do curso.

[![Image11](the-entity-framework-and-aspnet-getting-started-part-3/_static/image30.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image29.png)

Insira um "m" e clique em **Pesquisar** para ver todos os cursos cujos títulos comecem com "m" (a pesquisa não diferencia maiúsculas de minúsculas).

[![Image12](the-entity-framework-and-aspnet-getting-started-part-3/_static/image32.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image31.png)

## <a name="using-the-like-operator-to-filter-data"></a>Usando o operador "Like" para filtrar dados

Você pode obter um efeito semelhante aos tipos de pesquisa `StartsWith`, `Contains`e `EndsWith` do controle de `QueryExtender` usando um operador `Like` na propriedade `EntityDataSource` do controle `Where`. Nesta parte do tutorial, você verá como usar o operador de `Like` para procurar um estudante por nome.

Abra *students. aspx* na exibição de **código-fonte** . Após o controle de `GridView`, adicione a seguinte marcação:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-3/samples/sample9.aspx)]

Essa marcação é semelhante ao que você viu anteriormente, exceto pelo valor da propriedade `Where`. A segunda parte da expressão de `Where` define uma pesquisa de subcadeia de caracteres (`LIKE %FirstMidName% or LIKE %LastName%`) que pesquisa o nome e o sobrenome de qualquer coisa que seja inserida na caixa de texto.

Execute a página. Inicialmente, você vê todos os alunos porque o valor padrão para o parâmetro `StudentName` é "%".

[![Image13](the-entity-framework-and-aspnet-getting-started-part-3/_static/image34.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image33.png)

Insira a letra "g" na caixa de texto e clique em **Pesquisar**. Você verá uma lista de alunos que têm um "g" no nome ou sobrenome.

[![Image14](the-entity-framework-and-aspnet-getting-started-part-3/_static/image36.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image35.png)

Agora você exibiu, atualizou, filtrou, pediu e agrupava dados de tabelas individuais. No próximo tutorial, você começará a trabalhar com dados relacionados (cenários de detalhes mestre).

> [!div class="step-by-step"]
> [Anterior](the-entity-framework-and-aspnet-getting-started-part-2.md)
> [Próximo](the-entity-framework-and-aspnet-getting-started-part-4.md)
