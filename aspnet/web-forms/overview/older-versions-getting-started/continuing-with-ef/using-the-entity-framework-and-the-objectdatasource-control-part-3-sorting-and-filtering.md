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
# <a name="using-the-entity-framework-40-and-the-objectdatasource-control-part-3-sorting-and-filtering"></a><span data-ttu-id="8ebbf-104">Usando o Entity Framework 4,0 e o controle ObjectDataSource, parte 3: classificando e filtrando</span><span class="sxs-lookup"><span data-stu-id="8ebbf-104">Using the Entity Framework 4.0 and the ObjectDataSource Control, Part 3: Sorting and Filtering</span></span>

<span data-ttu-id="8ebbf-105">por [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="8ebbf-105">by [Tom Dykstra](https://github.com/tdykstra)</span></span>

> <span data-ttu-id="8ebbf-106">Esta série de tutoriais se baseia no aplicativo Web da Contoso University criado pelo [introdução com a série de tutoriais do Entity Framework 4,0](https://asp.net/entity-framework/tutorials#Getting%20Started) .</span><span class="sxs-lookup"><span data-stu-id="8ebbf-106">This tutorial series builds on the Contoso University web application that is created by the [Getting Started with the Entity Framework 4.0](https://asp.net/entity-framework/tutorials#Getting%20Started) tutorial series.</span></span> <span data-ttu-id="8ebbf-107">Se você não concluiu os tutoriais anteriores, como ponto de partida para este tutorial, você pode [baixar o aplicativo](https://code.msdn.microsoft.com/ASPNET-Web-Forms-97f8ee9a) que você criou.</span><span class="sxs-lookup"><span data-stu-id="8ebbf-107">If you didn't complete the earlier tutorials, as a starting point for this tutorial you can [download the application](https://code.msdn.microsoft.com/ASPNET-Web-Forms-97f8ee9a) that you would have created.</span></span> <span data-ttu-id="8ebbf-108">Você também pode [baixar o aplicativo](https://code.msdn.microsoft.com/ASPNET-Web-Forms-6c7197aa) que é criado pela série de tutoriais completa.</span><span class="sxs-lookup"><span data-stu-id="8ebbf-108">You can also [download the application](https://code.msdn.microsoft.com/ASPNET-Web-Forms-6c7197aa) that is created by the complete tutorial series.</span></span> <span data-ttu-id="8ebbf-109">Se você tiver dúvidas sobre os tutoriais, poderá lançá-los no [Fórum de Entity Framework ASP.net](https://forums.asp.net/1227.aspx).</span><span class="sxs-lookup"><span data-stu-id="8ebbf-109">If you have questions about the tutorials, you can post them to the [ASP.NET Entity Framework forum](https://forums.asp.net/1227.aspx).</span></span>

<span data-ttu-id="8ebbf-110">No tutorial anterior, você implementou o padrão de repositório em um aplicativo Web de n camadas que usa o Entity Framework e o controle de `ObjectDataSource`.</span><span class="sxs-lookup"><span data-stu-id="8ebbf-110">In the previous tutorial you implemented the repository pattern in an n-tier web application that uses the Entity Framework and the `ObjectDataSource` control.</span></span> <span data-ttu-id="8ebbf-111">Este tutorial mostra como fazer classificação e filtragem e lidar com cenários de detalhes mestres.</span><span class="sxs-lookup"><span data-stu-id="8ebbf-111">This tutorial shows how to do sorting and filtering and handle master-detail scenarios.</span></span> <span data-ttu-id="8ebbf-112">Você adicionará os seguintes aprimoramentos à página *departamentos. aspx* :</span><span class="sxs-lookup"><span data-stu-id="8ebbf-112">You'll add the following enhancements to the *Departments.aspx* page:</span></span>

- <span data-ttu-id="8ebbf-113">Uma caixa de texto para permitir que os usuários selecionem departamentos por nome.</span><span class="sxs-lookup"><span data-stu-id="8ebbf-113">A text box to allow users to select departments by name.</span></span>
- <span data-ttu-id="8ebbf-114">Uma lista de cursos para cada departamento que é mostrada na grade.</span><span class="sxs-lookup"><span data-stu-id="8ebbf-114">A list of courses for each department that's shown in the grid.</span></span>
- <span data-ttu-id="8ebbf-115">A capacidade de classificar clicando nos títulos das colunas.</span><span class="sxs-lookup"><span data-stu-id="8ebbf-115">The ability to sort by clicking column headings.</span></span>

<span data-ttu-id="8ebbf-116">[![Image01](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image2.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="8ebbf-116">[![Image01](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image2.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image1.png)</span></span>

## <a name="adding-the-ability-to-sort-gridview-columns"></a><span data-ttu-id="8ebbf-117">Adicionando a capacidade de classificar colunas GridView</span><span class="sxs-lookup"><span data-stu-id="8ebbf-117">Adding the Ability to Sort GridView Columns</span></span>

<span data-ttu-id="8ebbf-118">Abra a página Departments *. aspx* e adicione um atributo `SortParameterName="sortExpression"` ao controle de `ObjectDataSource` chamado `DepartmentsObjectDataSource`.</span><span class="sxs-lookup"><span data-stu-id="8ebbf-118">Open the *Departments.aspx* page and add a `SortParameterName="sortExpression"` attribute to the `ObjectDataSource` control named `DepartmentsObjectDataSource`.</span></span> <span data-ttu-id="8ebbf-119">(Posteriormente, você criará um método `GetDepartments` que usa um parâmetro chamado `sortExpression`.) A marcação para a marca de abertura do controle agora é semelhante ao exemplo a seguir.</span><span class="sxs-lookup"><span data-stu-id="8ebbf-119">(Later you'll create a `GetDepartments` method that takes a parameter named `sortExpression`.) The markup for the opening tag of the control now resembles the following example.</span></span>

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample1.aspx)]

<span data-ttu-id="8ebbf-120">Adicione o atributo `AllowSorting="true"` à marca de abertura do controle de `GridView`.</span><span class="sxs-lookup"><span data-stu-id="8ebbf-120">Add the `AllowSorting="true"` attribute to the opening tag of the `GridView` control.</span></span> <span data-ttu-id="8ebbf-121">A marcação para a marca de abertura do controle agora é semelhante ao exemplo a seguir.</span><span class="sxs-lookup"><span data-stu-id="8ebbf-121">The markup for the opening tag of the control now resembles the following example.</span></span>

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample2.aspx)]

<span data-ttu-id="8ebbf-122">No *Departments.aspx.cs*, defina a ordem de classificação padrão chamando o método `Sort` do controle de `GridView` do método `Page_Load`:</span><span class="sxs-lookup"><span data-stu-id="8ebbf-122">In *Departments.aspx.cs*, set the default sort order by calling the `GridView` control's `Sort` method from the `Page_Load` method:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample3.cs)]

<span data-ttu-id="8ebbf-123">Você pode adicionar um código que classifica ou filtra na classe de lógica de negócios ou na classe de repositório.</span><span class="sxs-lookup"><span data-stu-id="8ebbf-123">You can add code that sorts or filters in either the business logic class or the repository class.</span></span> <span data-ttu-id="8ebbf-124">Se você fizer isso na classe lógica de negócios, o trabalho de classificação ou filtragem será feito depois que os dados forem recuperados do banco, porque a classe lógica de negócios está trabalhando com um objeto `IEnumerable` retornado pelo repositório.</span><span class="sxs-lookup"><span data-stu-id="8ebbf-124">If you do it in the business logic class, the sorting or filtering work will be done after the data is retrieved from the database, because the business logic class is working with an `IEnumerable` object returned by the repository.</span></span> <span data-ttu-id="8ebbf-125">Se você adicionar código de classificação e filtragem na classe de repositório e fizer isso antes que uma expressão LINQ ou consulta de objeto tenha sido convertida em um objeto `IEnumerable`, seus comandos serão passados para o banco de dados para processamento, o que normalmente é mais eficiente.</span><span class="sxs-lookup"><span data-stu-id="8ebbf-125">If you add sorting and filtering code in the repository class and you do it before a LINQ expression or object query has been converted to an `IEnumerable` object, your commands will be passed through to the database for processing, which is typically more efficient.</span></span> <span data-ttu-id="8ebbf-126">Neste tutorial, você implementará a classificação e a filtragem de uma maneira que faz com que o processamento seja feito pelo banco de dados, ou seja, no repositório.</span><span class="sxs-lookup"><span data-stu-id="8ebbf-126">In this tutorial you'll implement sorting and filtering in a way that causes the processing to be done by the database — that is, in the repository.</span></span>

<span data-ttu-id="8ebbf-127">Para adicionar capacidade de classificação, você deve adicionar um novo método à interface de repositório e às classes de repositório, bem como à classe de lógica de negócios.</span><span class="sxs-lookup"><span data-stu-id="8ebbf-127">To add sorting capability, you must add a new method to the repository interface and repository classes as well as to the business logic class.</span></span> <span data-ttu-id="8ebbf-128">No arquivo *ISchoolRepository.cs* , adicione um novo método `GetDepartments` que usa um parâmetro `sortExpression` que será usado para classificar a lista de departamentos que é retornada:</span><span class="sxs-lookup"><span data-stu-id="8ebbf-128">In the *ISchoolRepository.cs* file, add a new `GetDepartments` method that takes a `sortExpression` parameter that will be used to sort the list of departments that's returned:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample4.cs)]

<span data-ttu-id="8ebbf-129">O parâmetro `sortExpression` especificará a coluna a ser classificada e a direção da classificação.</span><span class="sxs-lookup"><span data-stu-id="8ebbf-129">The `sortExpression` parameter will specify the column to sort on and the sort direction.</span></span>

<span data-ttu-id="8ebbf-130">Adicione o código para o novo método ao arquivo *SchoolRepository.cs* :</span><span class="sxs-lookup"><span data-stu-id="8ebbf-130">Add code for the new method to the *SchoolRepository.cs* file:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample5.cs)]

<span data-ttu-id="8ebbf-131">Altere o método de `GetDepartments` sem parâmetros existente para chamar o novo método:</span><span class="sxs-lookup"><span data-stu-id="8ebbf-131">Change the existing parameterless `GetDepartments` method to call the new method:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample6.cs)]

<span data-ttu-id="8ebbf-132">No projeto de teste, adicione o novo método a seguir para *MockSchoolRepository.cs*:</span><span class="sxs-lookup"><span data-stu-id="8ebbf-132">In the test project, add the following new method to *MockSchoolRepository.cs*:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample7.cs)]

<span data-ttu-id="8ebbf-133">Se você pretende criar qualquer teste de unidade que dependa desse método retornando uma lista classificada, precisaria classificar a lista antes de retorná-la.</span><span class="sxs-lookup"><span data-stu-id="8ebbf-133">If you were going to create any unit tests that depended on this method returning a sorted list, you would need to sort the list before returning it.</span></span> <span data-ttu-id="8ebbf-134">Você não criará testes como esse neste tutorial, portanto, o método pode simplesmente retornar a lista de departamentos não classificada.</span><span class="sxs-lookup"><span data-stu-id="8ebbf-134">You won't be creating tests like that in this tutorial, so the method can just return the unsorted list of departments.</span></span>

<span data-ttu-id="8ebbf-135">No arquivo *SchoolBL.cs* , adicione o seguinte novo método à classe lógica de negócios:</span><span class="sxs-lookup"><span data-stu-id="8ebbf-135">In the *SchoolBL.cs* file, add the following new method to the business logic class:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample8.cs)]

<span data-ttu-id="8ebbf-136">Esse código passa o parâmetro de classificação para o método Repository.</span><span class="sxs-lookup"><span data-stu-id="8ebbf-136">This code passes the sort parameter to the repository method.</span></span>

<span data-ttu-id="8ebbf-137">Execute a página *departamentos. aspx* .</span><span class="sxs-lookup"><span data-stu-id="8ebbf-137">Run the *Departments.aspx* page.</span></span>

<span data-ttu-id="8ebbf-138">[![Image02](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image4.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="8ebbf-138">[![Image02](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image4.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image3.png)</span></span>

<span data-ttu-id="8ebbf-139">Agora você pode clicar em qualquer título de coluna para classificar por essa coluna.</span><span class="sxs-lookup"><span data-stu-id="8ebbf-139">You can now click any column heading to sort by that column.</span></span> <span data-ttu-id="8ebbf-140">Se a coluna já estiver classificada, clicar no título inverterá a direção da classificação.</span><span class="sxs-lookup"><span data-stu-id="8ebbf-140">If the column is already sorted, clicking the heading reverses the sort direction.</span></span>

## <a name="adding-a-search-box"></a><span data-ttu-id="8ebbf-141">Adicionando uma caixa de pesquisa</span><span class="sxs-lookup"><span data-stu-id="8ebbf-141">Adding a Search Box</span></span>

<span data-ttu-id="8ebbf-142">Nesta seção, você adicionará uma caixa de texto de pesquisa, vinculará-a ao controle de `ObjectDataSource` usando um parâmetro de controle e adicionará um método à classe lógica de negócios para dar suporte à filtragem.</span><span class="sxs-lookup"><span data-stu-id="8ebbf-142">In this section you'll add a search text box, link it to the `ObjectDataSource` control using a control parameter, and add a method to the business logic class to support filtering.</span></span>

<span data-ttu-id="8ebbf-143">Abra a página Departments *. aspx* e adicione a seguinte marcação entre o cabeçalho e o primeiro controle de `ObjectDataSource`:</span><span class="sxs-lookup"><span data-stu-id="8ebbf-143">Open the *Departments.aspx* page and add the following markup between the heading and the first `ObjectDataSource` control:</span></span>

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample9.aspx)]

<span data-ttu-id="8ebbf-144">No controle de `ObjectDataSource` chamado `DepartmentsObjectDataSource`, faça o seguinte:</span><span class="sxs-lookup"><span data-stu-id="8ebbf-144">In the `ObjectDataSource` control named `DepartmentsObjectDataSource`, do the following:</span></span>

- <span data-ttu-id="8ebbf-145">Adicione um elemento `SelectParameters` para um parâmetro chamado `nameSearchString` que obtém o valor inserido no controle de `SearchTextBox`.</span><span class="sxs-lookup"><span data-stu-id="8ebbf-145">Add a `SelectParameters` element for a parameter named `nameSearchString` that gets the value entered in the `SearchTextBox` control.</span></span>
- <span data-ttu-id="8ebbf-146">Altere o valor do atributo `SelectMethod` para `GetDepartmentsByName`.</span><span class="sxs-lookup"><span data-stu-id="8ebbf-146">Change the `SelectMethod` attribute value to `GetDepartmentsByName`.</span></span> <span data-ttu-id="8ebbf-147">(Você criará esse método mais tarde.)</span><span class="sxs-lookup"><span data-stu-id="8ebbf-147">(You'll create this method later.)</span></span>

<span data-ttu-id="8ebbf-148">A marcação para o controle de `ObjectDataSource` agora é semelhante ao exemplo a seguir:</span><span class="sxs-lookup"><span data-stu-id="8ebbf-148">The markup for the `ObjectDataSource` control now resembles the following example:</span></span>

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample10.aspx)]

<span data-ttu-id="8ebbf-149">No *ISchoolRepository.cs*, adicione um método de `GetDepartmentsByName` que usa os parâmetros `sortExpression` e `nameSearchString`:</span><span class="sxs-lookup"><span data-stu-id="8ebbf-149">In *ISchoolRepository.cs*, add a `GetDepartmentsByName` method that takes both `sortExpression` and `nameSearchString` parameters:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample11.cs)]

<span data-ttu-id="8ebbf-150">No *SchoolRepository.cs*, adicione o seguinte novo método:</span><span class="sxs-lookup"><span data-stu-id="8ebbf-150">In *SchoolRepository.cs*, add the following new method:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample12.cs)]

<span data-ttu-id="8ebbf-151">Esse código usa um método `Where` para selecionar itens que contêm a cadeia de caracteres de pesquisa.</span><span class="sxs-lookup"><span data-stu-id="8ebbf-151">This code uses a `Where` method to select items that contain the search string.</span></span> <span data-ttu-id="8ebbf-152">Se a cadeia de caracteres de pesquisa estiver vazia, todos os registros serão selecionados.</span><span class="sxs-lookup"><span data-stu-id="8ebbf-152">If the search string is empty, all records will be selected.</span></span> <span data-ttu-id="8ebbf-153">Observe que, quando você especifica chamadas de método em uma instrução como esta (`Include`, `OrderBy`, depois `Where`), o método `Where` sempre deve ser o último.</span><span class="sxs-lookup"><span data-stu-id="8ebbf-153">Note that when you specify method calls together in one statement like this (`Include`, then `OrderBy`, then `Where`), the `Where` method must always be last.</span></span>

<span data-ttu-id="8ebbf-154">Altere o método de `GetDepartments` existente que usa um parâmetro `sortExpression` para chamar o novo método:</span><span class="sxs-lookup"><span data-stu-id="8ebbf-154">Change the existing `GetDepartments` method that takes a `sortExpression` parameter to call the new method:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample13.cs)]

<span data-ttu-id="8ebbf-155">No *MockSchoolRepository.cs* no projeto de teste, adicione o seguinte novo método:</span><span class="sxs-lookup"><span data-stu-id="8ebbf-155">In *MockSchoolRepository.cs* in the test project, add the following new method:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample14.cs)]

<span data-ttu-id="8ebbf-156">No *SchoolBL.cs*, adicione o seguinte novo método:</span><span class="sxs-lookup"><span data-stu-id="8ebbf-156">In *SchoolBL.cs*, add the following new method:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample15.cs)]

<span data-ttu-id="8ebbf-157">Execute a página Departments *. aspx* e insira uma cadeia de caracteres de pesquisa para certificar-se de que a lógica de seleção funciona.</span><span class="sxs-lookup"><span data-stu-id="8ebbf-157">Run the *Departments.aspx* page and enter a search string to make sure that the selection logic works.</span></span> <span data-ttu-id="8ebbf-158">Deixe a caixa de texto vazia e tente realizar uma pesquisa para certificar-se de que todos os registros sejam retornados.</span><span class="sxs-lookup"><span data-stu-id="8ebbf-158">Leave the text box empty and try a search to make sure that all records are returned.</span></span>

<span data-ttu-id="8ebbf-159">[![Image03](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image6.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="8ebbf-159">[![Image03](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image6.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image5.png)</span></span>

## <a name="adding-a-details-column-for-each-grid-row"></a><span data-ttu-id="8ebbf-160">Adicionando uma coluna de detalhes para cada linha de grade</span><span class="sxs-lookup"><span data-stu-id="8ebbf-160">Adding a Details Column for Each Grid Row</span></span>

<span data-ttu-id="8ebbf-161">Em seguida, você deseja ver todos os cursos de cada departamento exibidos na célula à direita da grade.</span><span class="sxs-lookup"><span data-stu-id="8ebbf-161">Next, you want to see all of the courses for each department displayed in the right-hand cell of the grid.</span></span> <span data-ttu-id="8ebbf-162">Para fazer isso, você usará um controle de `GridView` aninhado e o associará aos dados da propriedade de navegação `Courses` da entidade `Department`.</span><span class="sxs-lookup"><span data-stu-id="8ebbf-162">To do this, you'll use a nested `GridView` control and databind it to data from the `Courses` navigation property of the `Department` entity.</span></span>

<span data-ttu-id="8ebbf-163">Abra Departments *. aspx* e, na marcação do controle de `GridView`, especifique um manipulador para o evento `RowDataBound`.</span><span class="sxs-lookup"><span data-stu-id="8ebbf-163">Open *Departments.aspx* and in the markup for the `GridView` control, specify a handler for the `RowDataBound` event.</span></span> <span data-ttu-id="8ebbf-164">A marcação para a marca de abertura do controle agora é semelhante ao exemplo a seguir.</span><span class="sxs-lookup"><span data-stu-id="8ebbf-164">The markup for the opening tag of the control now resembles the following example.</span></span>

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample16.aspx)]

<span data-ttu-id="8ebbf-165">Adicione um novo elemento `TemplateField` após o campo `Administrator` Template:</span><span class="sxs-lookup"><span data-stu-id="8ebbf-165">Add a new `TemplateField` element after the `Administrator` template field:</span></span>

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample17.aspx)]

<span data-ttu-id="8ebbf-166">Essa marcação cria um controle de `GridView` aninhado que mostra o número do curso e o título de uma lista de cursos.</span><span class="sxs-lookup"><span data-stu-id="8ebbf-166">This markup creates a nested `GridView` control that shows the course number and title of a list of courses.</span></span> <span data-ttu-id="8ebbf-167">Ele não especifica uma fonte de dados porque você a associará ao código no manipulador de `RowDataBound`.</span><span class="sxs-lookup"><span data-stu-id="8ebbf-167">It does not specify a data source because you'll databind it in code in the `RowDataBound` handler.</span></span>

<span data-ttu-id="8ebbf-168">Abra *Departments.aspx.cs* e adicione o seguinte manipulador para o evento `RowDataBound`:</span><span class="sxs-lookup"><span data-stu-id="8ebbf-168">Open *Departments.aspx.cs* and add the following handler for the `RowDataBound` event:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample18.cs)]

<span data-ttu-id="8ebbf-169">Esse código obtém a entidade `Department` dos argumentos do evento, converte a propriedade de navegação `Courses` em uma coleção de `List` e vincula o `GridView` aninhado à coleção.</span><span class="sxs-lookup"><span data-stu-id="8ebbf-169">This code gets the `Department` entity from the event arguments, converts the `Courses` navigation property to a `List` collection, and databinds the nested `GridView` to the collection.</span></span>

<span data-ttu-id="8ebbf-170">Abra o arquivo *SchoolRepository.cs* e especifique o carregamento adiantado para a propriedade de navegação `Courses` chamando o método `Include` na consulta de objeto que você cria no método `GetDepartmentsByName`.</span><span class="sxs-lookup"><span data-stu-id="8ebbf-170">Open the *SchoolRepository.cs* file and specify eager loading for the `Courses` navigation property by calling the `Include` method in the object query that you create in the `GetDepartmentsByName` method.</span></span> <span data-ttu-id="8ebbf-171">A instrução `return` no método `GetDepartmentsByName` agora é semelhante ao exemplo a seguir.</span><span class="sxs-lookup"><span data-stu-id="8ebbf-171">The `return` statement in the `GetDepartmentsByName` method now resembles the following example.</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample19.cs)]

<span data-ttu-id="8ebbf-172">Execute a página.</span><span class="sxs-lookup"><span data-stu-id="8ebbf-172">Run the page.</span></span> <span data-ttu-id="8ebbf-173">Além do recurso de classificação e filtragem que você adicionou anteriormente, o controle GridView agora mostra detalhes de curso aninhados para cada departamento.</span><span class="sxs-lookup"><span data-stu-id="8ebbf-173">In addition to the sorting and filtering capability that you added earlier, the GridView control now shows nested course details for each department.</span></span>

<span data-ttu-id="8ebbf-174">[![Image01](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image8.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="8ebbf-174">[![Image01](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image8.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image7.png)</span></span>

<span data-ttu-id="8ebbf-175">Isso conclui a introdução aos cenários de classificação, filtragem e detalhes mestre.</span><span class="sxs-lookup"><span data-stu-id="8ebbf-175">This completes the introduction to sorting, filtering, and master-detail scenarios.</span></span> <span data-ttu-id="8ebbf-176">No próximo tutorial, você verá como lidar com a simultaneidade.</span><span class="sxs-lookup"><span data-stu-id="8ebbf-176">In the next tutorial, you'll see how to handle concurrency.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="8ebbf-177">[Anterior](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests.md)
> [Próximo](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application.md)</span><span class="sxs-lookup"><span data-stu-id="8ebbf-177">[Previous](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests.md)
[Next](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application.md)</span></span>
