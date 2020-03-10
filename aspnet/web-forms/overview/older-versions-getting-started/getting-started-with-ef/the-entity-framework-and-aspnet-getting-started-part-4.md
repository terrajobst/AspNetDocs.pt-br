---
uid: web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-4
title: Introdução com Entity Framework 4,0 Database First e ASP.NET 4 Web Forms-parte 4 | Microsoft Docs
author: tdykstra
description: O aplicativo Web de exemplo Contoso University demonstra como criar ASP.NET Web Forms aplicativos usando o Entity Framework. O aplicativo de exemplo é...
ms.author: riande
ms.date: 12/03/2010
ms.assetid: ceb9e60f-957c-4d25-9331-cc527de96a33
msc.legacyurl: /web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-4
msc.type: authoredcontent
ms.openlocfilehash: eb75a76038466bf30738387ed4739687de1df944
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78638162"
---
# <a name="getting-started-with-entity-framework-40-database-first-and-aspnet-4-web-forms---part-4"></a>Introdução com Entity Framework 4,0 Database First e ASP.NET 4 Web Forms-parte 4

por [Tom Dykstra](https://github.com/tdykstra)

> O aplicativo Web de exemplo Contoso University demonstra como criar ASP.NET Web Forms aplicativos usando o Entity Framework 4,0 e o Visual Studio 2010. Para obter informações sobre a série de tutoriais, consulte [o primeiro tutorial da série](the-entity-framework-and-aspnet-getting-started-part-1.md)

## <a name="working-with-related-data"></a>Trabalhando com dados relacionados

No tutorial anterior, você usou o controle de `EntityDataSource` para filtrar, classificar e agrupar dados. Neste tutorial, você exibirá e atualizará os dados relacionados.

Você criará a página instrutores que mostra uma lista de instrutores. Ao selecionar um instrutor, você verá uma lista de cursos ensinados por esse instrutor. Ao selecionar um curso, você verá os detalhes do curso e uma lista de alunos registrados no curso. Você pode editar o nome do instrutor, a data de contratação e a atribuição do Office. A atribuição do Office é um conjunto de entidades separado que você acessa por meio de uma propriedade de navegação.

Você pode vincular dados mestres a dados detalhados na marcação ou no código. Nesta parte do tutorial, você usará os dois métodos.

[![Image01](the-entity-framework-and-aspnet-getting-started-part-4/_static/image2.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image1.png)

## <a name="displaying-and-updating-related-entities-in-a-gridview-control"></a>Exibindo e atualizando entidades relacionadas em um controle GridView

Crie uma nova página da Web chamada *instrutores. aspx* que usa a página mestra *site. Master* e adicione a marcação a seguir ao controle de `Content` chamado `Content2`:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample1.aspx)]

Essa marcação cria um controle de `EntityDataSource` que seleciona instrutores e habilita atualizações. O elemento `div` configura a marcação a ser renderizada à esquerda para que você possa adicionar uma coluna à direita posteriormente.

Entre a marcação de `EntityDataSource` e a marca de `</div>` de fechamento, adicione a seguinte marcação que cria um controle de `GridView` e um controle de `Label` que você usará para mensagens de erro:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample2.aspx)]

Este controle de `GridView` habilita a seleção de linha, realça a linha selecionada com uma cor de fundo cinza-clara e especifica manipuladores (que você criará posteriormente) para os eventos `SelectedIndexChanged` e `Updating`. Ele também especifica `PersonID` para a propriedade `DataKeyNames`, para que o valor de chave da linha selecionada possa ser passado para outro controle que você adicionará posteriormente.

A última coluna contém a atribuição de escritório do instrutor, que é armazenada em uma propriedade de navegação da entidade `Person` porque ela vem de uma entidade associada. Observe que o elemento `EditItemTemplate` especifica `Eval` em vez de `Bind`, porque o controle de `GridView` não pode se associar diretamente a propriedades de navegação para atualizá-los. Você atualizará a atribuição do Office no código. Para fazer isso, você precisará de uma referência ao controle de `TextBox` e irá salvá-lo no evento de `Init` do controle de `TextBox`.

Seguir o controle de `GridView` é um controle de `Label` usado para mensagens de erro. A propriedade `Visible` do controle é `false`e o estado de exibição é desativado, de modo que o rótulo será exibido somente quando o código torná-lo visível em resposta a um erro.

Abra o arquivo *Instructors.aspx.cs* e adicione a seguinte instrução de `using`:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample3.cs)]

Adicione um campo de classe particular imediatamente após a declaração de nome de classe parcial para manter uma referência à caixa de texto de atribuição do Office.

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample4.cs)]

Adicione um stub para o manipulador de eventos `SelectedIndexChanged` para o qual você adicionará o código posteriormente. Além disso, adicione um manipulador para o evento de `Init` do controle de `TextBox` de atribuição do Office para que você possa armazenar uma referência ao controle de `TextBox`. Você usará essa referência para obter o valor inserido pelo usuário para atualizar a entidade associada à propriedade de navegação.

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample5.cs)]

Você usará o evento `Updating` do controle de `GridView` para atualizar a propriedade `Location` da entidade `OfficeAssignment` associada. Adicione o seguinte manipulador para o evento de `Updating`:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample6.cs)]

Esse código é executado quando o usuário clica em **Atualizar** em uma linha `GridView`. O código usa LINQ to Entities para recuperar a entidade de `OfficeAssignment` associada à entidade de `Person` atual, usando o `PersonID` da linha selecionada a partir do argumento de evento.

Em seguida, o código usa uma das seguintes ações dependendo do valor no controle de `InstructorOfficeTextBox`:

- Se a caixa de texto tiver um valor e não houver nenhuma entidade `OfficeAssignment` para atualizar, ele criará um.
- Se a caixa de texto tiver um valor e houver uma entidade `OfficeAssignment`, ela atualizará o valor da propriedade `Location`.
- Se a caixa de texto estiver vazia e existir uma `OfficeAssignment` entidade, ela excluirá a entidade.

Depois disso, ele salva as alterações no banco de dados. Se ocorrer uma exceção, ele exibirá uma mensagem de erro.

Execute a página.

[![Image02](the-entity-framework-and-aspnet-getting-started-part-4/_static/image4.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image3.png)

Clique em **Editar** e todos os campos são alterados para as caixas de texto.

[![Image03](the-entity-framework-and-aspnet-getting-started-part-4/_static/image6.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image5.png)

Altere qualquer um desses valores, incluindo a **atribuição do Office**. Clique em **Atualizar** e você verá as alterações refletidas na lista.

## <a name="displaying-related-entities-in-a-separate-control"></a>Exibindo entidades relacionadas em um controle separado

Cada instrutor pode ensinar um ou mais cursos, portanto, você adicionará um controle de `EntityDataSource` e um controle de `GridView` para listar os cursos associados a qualquer instrutor selecionado no controle instrutores `GridView`. Para criar um título e o controle de `EntityDataSource` para entidades de cursos, adicione a seguinte marcação entre a mensagem de erro `Label` controle e a marca de `</div>` de fechamento:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample7.aspx)]

O parâmetro `Where` contém o valor da `PersonID` do instrutor cuja linha está selecionada no controle de `InstructorsGridView`. A propriedade `Where` contém um comando de subseleção que obtém todas as entidades de `Person` associadas da propriedade de navegação `People` de uma entidade `Course` e seleciona a entidade de `Course` somente se uma das entidades de `Person` associadas contiver o valor de `PersonID` selecionado.

Para criar o controle de `GridView`., adicione a seguinte marcação imediatamente após o controle de `CoursesEntityDataSource` (antes da marca de fechamento `</div>`):

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample8.aspx)]

Como nenhum curso será exibido se nenhum instrutor for selecionado, um elemento `EmptyDataTemplate` será incluído.

Execute a página.

[![Image04](the-entity-framework-and-aspnet-getting-started-part-4/_static/image8.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image7.png)

Selecione um instrutor que tenha um ou mais cursos atribuídos e o curso ou os cursos apareçam na lista. (Observação: apesar de o esquema de banco de dados permitir vários cursos, nos dados de teste fornecidos com o Database, nenhum instrutor tem mais de um curso. Você mesmo pode adicionar cursos ao banco de dados usando a janela **Gerenciador de servidores** ou a página *CoursesAdd. aspx* , que você adicionará em um tutorial posterior.)

[![Image05](the-entity-framework-and-aspnet-getting-started-part-4/_static/image10.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image9.png)

O controle de `CoursesGridView` mostra apenas alguns campos de curso. Para exibir todos os detalhes de um curso, você usará um controle de `DetailsView` para o curso que o usuário seleciona. Em *instrutores. aspx*, adicione a marcação a seguir após a marcação de `</div>` de fechamento (certifique-se de colocar essa marcação **após** a marca DIV de fechamento, não antes dela):

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample9.aspx)]

Essa marcação cria um controle `EntityDataSource` associado ao conjunto de entidades `Courses`. A propriedade `Where` seleciona um curso usando o valor de `CourseID` da linha selecionada no controle cursos `GridView`. A marcação Especifica um manipulador para o evento `Selected`, que você usará posteriormente para exibir notas de alunos, que é outro nível inferior na hierarquia.

No *Instructors.aspx.cs*, crie o seguinte stub para o método `CourseDetailsEntityDataSource_Selected`. (Você preencherá esse stub posteriormente no tutorial; por enquanto, você precisará dele para que a página seja compilada e executada.)

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample10.cs)]

Execute a página.

[![Image06](the-entity-framework-and-aspnet-getting-started-part-4/_static/image12.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image11.png)

Inicialmente, não há detalhes do curso porque nenhum curso está selecionado. Selecione um instrutor que tenha um curso atribuído e, em seguida, selecione um curso para ver os detalhes.

[![Image07](the-entity-framework-and-aspnet-getting-started-part-4/_static/image14.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image13.png)

## <a name="using-the-entitydatasource-selected-event-to-display-related-data"></a>Usando o evento de EntityDataSource "selecionado" para exibir dados relacionados

Por fim, você deseja mostrar todos os alunos registrados e suas notas para o curso selecionado. Para fazer isso, você usará o evento `Selected` do controle de `EntityDataSource` associado ao `DetailsView`do curso.

Em *instrutores. aspx*, adicione a seguinte marcação após o controle de `DetailsView`:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample11.aspx)]

Essa marcação cria um controle de `ListView` que exibe uma lista de alunos e suas notas para o curso selecionado. Nenhuma fonte de dados foi especificada porque você associará o controle no código. O elemento `EmptyDataTemplate` fornece uma mensagem a ser exibida quando nenhum curso é selecionado — nesse caso, não há alunos a serem exibidos. O elemento `LayoutTemplate` cria uma tabela HTML para exibir a lista e o `ItemTemplate` especifica as colunas a serem exibidas. A ID do estudante e a classificação do aluno são da entidade `StudentGrade`, e o nome do aluno é da entidade `Person` que o Entity Framework disponibiliza na propriedade de navegação `Person` da entidade `StudentGrade`.

No *Instructors.aspx.cs*, substitua o método de `CourseDetailsEntityDataSource_Selected` fragmentado pelo código a seguir:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample12.cs)]

O argumento de evento para esse evento fornece os dados selecionados na forma de uma coleção, que terá zero itens se nada for selecionado ou um item se uma entidade `Course` for selecionada. Se uma entidade `Course` for selecionada, o código usará o método `First` para converter a coleção em um único objeto. Em seguida, ele obtém `StudentGrade` entidades da propriedade de navegação, converte-as em uma coleção e associa o controle de `GradesListView` à coleção.

Isso é suficiente para exibir notas, mas você deseja ter certeza de que a mensagem no modelo de dados vazio é exibida na primeira vez em que a página é exibida e sempre que um curso não é selecionado. Para fazer isso, crie o seguinte método, que você chamará de dois locais:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample13.cs)]

Chame esse novo método do método `Page_Load` para exibir o modelo de dados vazio na primeira vez em que a página for exibida. E chamá-lo do método `InstructorsGridView_SelectedIndexChanged` porque esse evento é gerado quando um instrutor é selecionado, o que significa que novos cursos são carregados no controle de cursos `GridView` e nenhum está selecionado ainda. Aqui estão as duas chamadas:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample14.cs)]

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample15.cs)]

Execute a página.

[![Image08](the-entity-framework-and-aspnet-getting-started-part-4/_static/image16.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image15.png)

Selecione um instrutor que tenha um curso atribuído e, em seguida, selecione o curso.

[![Image09](the-entity-framework-and-aspnet-getting-started-part-4/_static/image18.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image17.png)

Agora você viu algumas maneiras de trabalhar com dados relacionados. No tutorial a seguir, você aprenderá a adicionar relações entre entidades existentes, como remover relações e como adicionar uma nova entidade que tem uma relação com uma entidade existente.

> [!div class="step-by-step"]
> [Anterior](the-entity-framework-and-aspnet-getting-started-part-3.md)
> [Próximo](the-entity-framework-and-aspnet-getting-started-part-5.md)
