---
uid: web-forms/overview/older-versions-getting-started/continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering
title: 'Usando o Entity Framework 4,0 e o controle ObjectDataSource, parte 3: classificando e filtrando | Microsoft Docs'
author: tdykstra
description: Esta série de tutoriais se baseia no aplicativo Web da Contoso University criado pelo Introdução com a série de tutoriais do Entity Framework 4,0. I...
ms.author: riande
ms.date: 01/26/2011
ms.assetid: 2990bd10-590d-43d5-9529-6b503ce5455d
msc.legacyurl: /web-forms/overview/older-versions-getting-started/continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering
msc.type: authoredcontent
ms.openlocfilehash: 603120864528b9a5ff81214270eb9a7f1b68b347
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78631666"
---
# <a name="using-the-entity-framework-40-and-the-objectdatasource-control-part-3-sorting-and-filtering"></a>Usando o Entity Framework 4,0 e o controle ObjectDataSource, parte 3: classificando e filtrando

por [Tom Dykstra](https://github.com/tdykstra)

> Esta série de tutoriais se baseia no aplicativo Web da Contoso University criado pelo [introdução com a série de tutoriais do Entity Framework 4,0](https://asp.net/entity-framework/tutorials#Getting%20Started) . Se você não concluiu os tutoriais anteriores, como ponto de partida para este tutorial, você pode [baixar o aplicativo](https://code.msdn.microsoft.com/ASPNET-Web-Forms-97f8ee9a) que você criou. Você também pode [baixar o aplicativo](https://code.msdn.microsoft.com/ASPNET-Web-Forms-6c7197aa) que é criado pela série de tutoriais completa. Se você tiver dúvidas sobre os tutoriais, poderá lançá-los no [Fórum de Entity Framework ASP.net](https://forums.asp.net/1227.aspx).

No tutorial anterior, você implementou o padrão de repositório em um aplicativo Web de n camadas que usa o Entity Framework e o controle de `ObjectDataSource`. Este tutorial mostra como fazer classificação e filtragem e lidar com cenários de detalhes mestres. Você adicionará os seguintes aprimoramentos à página *departamentos. aspx* :

- Uma caixa de texto para permitir que os usuários selecionem departamentos por nome.
- Uma lista de cursos para cada departamento que é mostrada na grade.
- A capacidade de classificar clicando nos títulos das colunas.

[![Image01](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image2.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image1.png)

## <a name="adding-the-ability-to-sort-gridview-columns"></a>Adicionando a capacidade de classificar colunas GridView

Abra a página Departments *. aspx* e adicione um atributo `SortParameterName="sortExpression"` ao controle de `ObjectDataSource` chamado `DepartmentsObjectDataSource`. (Posteriormente, você criará um método `GetDepartments` que usa um parâmetro chamado `sortExpression`.) A marcação para a marca de abertura do controle agora é semelhante ao exemplo a seguir.

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample1.aspx)]

Adicione o atributo `AllowSorting="true"` à marca de abertura do controle de `GridView`. A marcação para a marca de abertura do controle agora é semelhante ao exemplo a seguir.

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample2.aspx)]

No *Departments.aspx.cs*, defina a ordem de classificação padrão chamando o método `Sort` do controle de `GridView` do método `Page_Load`:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample3.cs)]

Você pode adicionar um código que classifica ou filtra na classe de lógica de negócios ou na classe de repositório. Se você fizer isso na classe lógica de negócios, o trabalho de classificação ou filtragem será feito depois que os dados forem recuperados do banco, porque a classe lógica de negócios está trabalhando com um objeto `IEnumerable` retornado pelo repositório. Se você adicionar código de classificação e filtragem na classe de repositório e fizer isso antes que uma expressão LINQ ou consulta de objeto tenha sido convertida em um objeto `IEnumerable`, seus comandos serão passados para o banco de dados para processamento, o que normalmente é mais eficiente. Neste tutorial, você implementará a classificação e a filtragem de uma maneira que faz com que o processamento seja feito pelo banco de dados, ou seja, no repositório.

Para adicionar capacidade de classificação, você deve adicionar um novo método à interface de repositório e às classes de repositório, bem como à classe de lógica de negócios. No arquivo *ISchoolRepository.cs* , adicione um novo método `GetDepartments` que usa um parâmetro `sortExpression` que será usado para classificar a lista de departamentos que é retornada:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample4.cs)]

O parâmetro `sortExpression` especificará a coluna a ser classificada e a direção da classificação.

Adicione o código para o novo método ao arquivo *SchoolRepository.cs* :

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample5.cs)]

Altere o método de `GetDepartments` sem parâmetros existente para chamar o novo método:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample6.cs)]

No projeto de teste, adicione o novo método a seguir para *MockSchoolRepository.cs*:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample7.cs)]

Se você pretende criar qualquer teste de unidade que dependa desse método retornando uma lista classificada, precisaria classificar a lista antes de retorná-la. Você não criará testes como esse neste tutorial, portanto, o método pode simplesmente retornar a lista de departamentos não classificada.

No arquivo *SchoolBL.cs* , adicione o seguinte novo método à classe lógica de negócios:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample8.cs)]

Esse código passa o parâmetro de classificação para o método Repository.

Execute a página *departamentos. aspx* .

[![Image02](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image4.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image3.png)

Agora você pode clicar em qualquer título de coluna para classificar por essa coluna. Se a coluna já estiver classificada, clicar no título inverterá a direção da classificação.

## <a name="adding-a-search-box"></a>Adicionando uma caixa de pesquisa

Nesta seção, você adicionará uma caixa de texto de pesquisa, vinculará-a ao controle de `ObjectDataSource` usando um parâmetro de controle e adicionará um método à classe lógica de negócios para dar suporte à filtragem.

Abra a página Departments *. aspx* e adicione a seguinte marcação entre o cabeçalho e o primeiro controle de `ObjectDataSource`:

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample9.aspx)]

No controle de `ObjectDataSource` chamado `DepartmentsObjectDataSource`, faça o seguinte:

- Adicione um elemento `SelectParameters` para um parâmetro chamado `nameSearchString` que obtém o valor inserido no controle de `SearchTextBox`.
- Altere o valor do atributo `SelectMethod` para `GetDepartmentsByName`. (Você criará esse método mais tarde.)

A marcação para o controle de `ObjectDataSource` agora é semelhante ao exemplo a seguir:

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample10.aspx)]

No *ISchoolRepository.cs*, adicione um método de `GetDepartmentsByName` que usa os parâmetros `sortExpression` e `nameSearchString`:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample11.cs)]

No *SchoolRepository.cs*, adicione o seguinte novo método:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample12.cs)]

Esse código usa um método `Where` para selecionar itens que contêm a cadeia de caracteres de pesquisa. Se a cadeia de caracteres de pesquisa estiver vazia, todos os registros serão selecionados. Observe que, quando você especifica chamadas de método em uma instrução como esta (`Include`, `OrderBy`, depois `Where`), o método `Where` sempre deve ser o último.

Altere o método de `GetDepartments` existente que usa um parâmetro `sortExpression` para chamar o novo método:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample13.cs)]

No *MockSchoolRepository.cs* no projeto de teste, adicione o seguinte novo método:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample14.cs)]

No *SchoolBL.cs*, adicione o seguinte novo método:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample15.cs)]

Execute a página Departments *. aspx* e insira uma cadeia de caracteres de pesquisa para certificar-se de que a lógica de seleção funciona. Deixe a caixa de texto vazia e tente realizar uma pesquisa para certificar-se de que todos os registros sejam retornados.

[![Image03](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image6.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image5.png)

## <a name="adding-a-details-column-for-each-grid-row"></a>Adicionando uma coluna de detalhes para cada linha de grade

Em seguida, você deseja ver todos os cursos de cada departamento exibidos na célula à direita da grade. Para fazer isso, você usará um controle de `GridView` aninhado e o associará aos dados da propriedade de navegação `Courses` da entidade `Department`.

Abra Departments *. aspx* e, na marcação do controle de `GridView`, especifique um manipulador para o evento `RowDataBound`. A marcação para a marca de abertura do controle agora é semelhante ao exemplo a seguir.

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample16.aspx)]

Adicione um novo elemento `TemplateField` após o campo `Administrator` Template:

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample17.aspx)]

Essa marcação cria um controle de `GridView` aninhado que mostra o número do curso e o título de uma lista de cursos. Ele não especifica uma fonte de dados porque você a associará ao código no manipulador de `RowDataBound`.

Abra *Departments.aspx.cs* e adicione o seguinte manipulador para o evento `RowDataBound`:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample18.cs)]

Esse código obtém a entidade `Department` dos argumentos do evento, converte a propriedade de navegação `Courses` em uma coleção de `List` e vincula o `GridView` aninhado à coleção.

Abra o arquivo *SchoolRepository.cs* e especifique o carregamento adiantado para a propriedade de navegação `Courses` chamando o método `Include` na consulta de objeto que você cria no método `GetDepartmentsByName`. A instrução `return` no método `GetDepartmentsByName` agora é semelhante ao exemplo a seguir.

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample19.cs)]

Execute a página. Além do recurso de classificação e filtragem que você adicionou anteriormente, o controle GridView agora mostra detalhes de curso aninhados para cada departamento.

[![Image01](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image8.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image7.png)

Isso conclui a introdução aos cenários de classificação, filtragem e detalhes mestre. No próximo tutorial, você verá como lidar com a simultaneidade.

> [!div class="step-by-step"]
> [Anterior](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests.md)
> [Próximo](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application.md)
