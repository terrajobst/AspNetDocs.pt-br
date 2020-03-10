---
uid: web-forms/overview/presenting-and-managing-data/model-binding/sorting-paging-and-filtering-data
title: Classificando, paginando e Filtrando dados com associação de modelo e formulários da Web | Microsoft Docs
author: Rick-Anderson
description: Esta série de tutoriais demonstra os aspectos básicos do uso de associação de modelo com um projeto ASP.NET Web Forms. A associação de modelo torna a interação de dados mais direta-...
ms.author: riande
ms.date: 02/27/2014
ms.assetid: 266e7866-e327-4687-b29d-627a0925e87d
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/sorting-paging-and-filtering-data
msc.type: authoredcontent
ms.openlocfilehash: f8e64392af6110f36c6af98c4e4e9481c94a0d82
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78548058"
---
# <a name="sorting-paging-and-filtering-data-with-model-binding-and-web-forms"></a><span data-ttu-id="9384a-104">Classificando, paginando e Filtrando dados com associação de modelo e formulários da Web</span><span class="sxs-lookup"><span data-stu-id="9384a-104">Sorting, paging, and filtering data with model binding and web forms</span></span>

<span data-ttu-id="9384a-105">por [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="9384a-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="9384a-106">Esta série de tutoriais demonstra os aspectos básicos do uso de associação de modelo com um projeto ASP.NET Web Forms.</span><span class="sxs-lookup"><span data-stu-id="9384a-106">This tutorial series demonstrates basic aspects of using model binding with an ASP.NET Web Forms project.</span></span> <span data-ttu-id="9384a-107">A associação de modelo torna a interação de dados mais direta do que lidar com objetos de fonte de dados (como ObjectDataSource ou SqlDataSource).</span><span class="sxs-lookup"><span data-stu-id="9384a-107">Model binding makes data interaction more straight-forward than dealing with data source objects (such as ObjectDataSource or SqlDataSource).</span></span> <span data-ttu-id="9384a-108">Esta série começa com material introdutório e passa para conceitos mais avançados em Tutoriais posteriores.</span><span class="sxs-lookup"><span data-stu-id="9384a-108">This series starts with introductory material and moves to more advanced concepts in later tutorials.</span></span>
> 
> <span data-ttu-id="9384a-109">Este tutorial mostra como adicionar classificação, paginação e filtragem dos dados por meio de associação de modelo.</span><span class="sxs-lookup"><span data-stu-id="9384a-109">This tutorial shows how to add sorting, paging, and filtering of the data through model binding.</span></span>
> 
> <span data-ttu-id="9384a-110">Este tutorial se baseia no projeto criado na primeira [parte](retrieving-data.md) da série.</span><span class="sxs-lookup"><span data-stu-id="9384a-110">This tutorial builds on the project created in the first [part](retrieving-data.md) of the series.</span></span>
> 
> <span data-ttu-id="9384a-111">Você pode [baixar](https://go.microsoft.com/fwlink/?LinkId=286116) o projeto completo no C# ou no VB.</span><span class="sxs-lookup"><span data-stu-id="9384a-111">You can [download](https://go.microsoft.com/fwlink/?LinkId=286116) the complete project in C# or VB.</span></span> <span data-ttu-id="9384a-112">O código para download funciona com o Visual Studio 2012 ou Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="9384a-112">The downloadable code works with either Visual Studio 2012 or Visual Studio 2013.</span></span> <span data-ttu-id="9384a-113">Ele usa o modelo do Visual Studio 2012, que é um pouco diferente do modelo de Visual Studio 2013 mostrado neste tutorial.</span><span class="sxs-lookup"><span data-stu-id="9384a-113">It uses the Visual Studio 2012 template, which is slightly different than the Visual Studio 2013 template shown in this tutorial.</span></span>

## <a name="what-youll-build"></a><span data-ttu-id="9384a-114">O que você criará</span><span class="sxs-lookup"><span data-stu-id="9384a-114">What you'll build</span></span>

<span data-ttu-id="9384a-115">Neste tutorial, você aprenderá a:</span><span class="sxs-lookup"><span data-stu-id="9384a-115">In this tutorial, you'll:</span></span>

1. <span data-ttu-id="9384a-116">Habilitar classificação e paginação dos dados</span><span class="sxs-lookup"><span data-stu-id="9384a-116">Enable sorting and paging of the data</span></span>
2. <span data-ttu-id="9384a-117">Habilitar a filtragem dos dados com base em uma seleção pelo usuário</span><span class="sxs-lookup"><span data-stu-id="9384a-117">Enable filtering of the data based on a selection by the user</span></span>

## <a name="add-sorting"></a><span data-ttu-id="9384a-118">Adicionar classificação</span><span class="sxs-lookup"><span data-stu-id="9384a-118">Add sorting</span></span>

<span data-ttu-id="9384a-119">Habilitar a classificação no GridView é muito fácil.</span><span class="sxs-lookup"><span data-stu-id="9384a-119">Enabling sorting in the GridView is very easy.</span></span> <span data-ttu-id="9384a-120">No arquivo Student. aspx, basta definir **AllowSorting** como **true** no GridView.</span><span class="sxs-lookup"><span data-stu-id="9384a-120">In the Student.aspx file, simply set **AllowSorting** to **true** in the GridView.</span></span> <span data-ttu-id="9384a-121">Você não precisa definir um valor de **SortExpression** para cada coluna, pois DataField é usado automaticamente.</span><span class="sxs-lookup"><span data-stu-id="9384a-121">You do not need to set a **SortExpression** value for each column as the DataField is automatically used.</span></span> <span data-ttu-id="9384a-122">O GridView modifica a consulta para incluir a ordenação dos dados pelo valor selecionado.</span><span class="sxs-lookup"><span data-stu-id="9384a-122">The GridView modifies the query to include ordering the data by the selected value.</span></span> <span data-ttu-id="9384a-123">O código realçado abaixo mostra a adição que você precisa fazer para habilitar a classificação.</span><span class="sxs-lookup"><span data-stu-id="9384a-123">The highlighted code below shows the addition you need to make to enable sorting.</span></span>

[!code-aspx[Main](sorting-paging-and-filtering-data/samples/sample1.aspx?highlight=5)]

<span data-ttu-id="9384a-124">Execute o aplicativo Web e teste classificando os registros de aluno pelos valores em colunas diferentes.</span><span class="sxs-lookup"><span data-stu-id="9384a-124">Run the web application, and test sorting student records by the values in different columns.</span></span>

![classificar alunos](sorting-paging-and-filtering-data/_static/image2.png)

## <a name="add-paging"></a><span data-ttu-id="9384a-126">Adicionar paginação</span><span class="sxs-lookup"><span data-stu-id="9384a-126">Add paging</span></span>

<span data-ttu-id="9384a-127">Habilitar a paginação também é muito fácil.</span><span class="sxs-lookup"><span data-stu-id="9384a-127">Enabling paging is also very easy.</span></span> <span data-ttu-id="9384a-128">No GridView, defina a propriedade **AllowPaging** como **true** e defina a propriedade **PageSize** como o número de registros que você deseja exibir em cada página.</span><span class="sxs-lookup"><span data-stu-id="9384a-128">In the GridView, set the **AllowPaging** property to **true** and set the **PageSize** property to the number of records you wish to display on each page.</span></span> <span data-ttu-id="9384a-129">Neste tutorial, você pode defini-lo como 4.</span><span class="sxs-lookup"><span data-stu-id="9384a-129">In this tutorial, you can set it to 4.</span></span>

[!code-aspx[Main](sorting-paging-and-filtering-data/samples/sample2.aspx?highlight=5)]

<span data-ttu-id="9384a-130">Execute o aplicativo Web e observe que agora os registros são divididos em várias páginas com até 4 registros exibidos em uma única página.</span><span class="sxs-lookup"><span data-stu-id="9384a-130">Run the web application, and notice that now the records are divided over multiple pages with no more than 4 records displayed on a single page.</span></span>

![adicionar paginação](sorting-paging-and-filtering-data/_static/image4.png)

<span data-ttu-id="9384a-132">A execução da consulta adiada melhora a eficiência do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="9384a-132">Deferred query execution improves the application efficiency.</span></span> <span data-ttu-id="9384a-133">Em vez de recuperar todo o conjunto de dados, o GridView modifica a consulta para recuperar somente os registros da página atual.</span><span class="sxs-lookup"><span data-stu-id="9384a-133">Instead of retrieving the entire data set, the GridView modifies the query to retrieve only the records for the current page.</span></span>

## <a name="filter-records-by-user-selection"></a><span data-ttu-id="9384a-134">Filtrar registros por seleção de usuário</span><span class="sxs-lookup"><span data-stu-id="9384a-134">Filter records by user selection</span></span>

<span data-ttu-id="9384a-135">A associação de modelo adiciona vários atributos que permitem designar como definir o valor de um parâmetro em um método de associação de modelo.</span><span class="sxs-lookup"><span data-stu-id="9384a-135">Model binding adds several attributes which enable you to designate how to set the value for a parameter in a model binding method.</span></span> <span data-ttu-id="9384a-136">Esses atributos estão no namespace **System. Web. modelbinding** .</span><span class="sxs-lookup"><span data-stu-id="9384a-136">These attributes are in the **System.Web.ModelBinding** namespace.</span></span> <span data-ttu-id="9384a-137">Elas incluem:</span><span class="sxs-lookup"><span data-stu-id="9384a-137">They include:</span></span>

- <span data-ttu-id="9384a-138">Control</span><span class="sxs-lookup"><span data-stu-id="9384a-138">Control</span></span>
- <span data-ttu-id="9384a-139">Cookie</span><span class="sxs-lookup"><span data-stu-id="9384a-139">Cookie</span></span>
- <span data-ttu-id="9384a-140">Formulário</span><span class="sxs-lookup"><span data-stu-id="9384a-140">Form</span></span>
- <span data-ttu-id="9384a-141">Perfil</span><span class="sxs-lookup"><span data-stu-id="9384a-141">Profile</span></span>
- <span data-ttu-id="9384a-142">QueryString</span><span class="sxs-lookup"><span data-stu-id="9384a-142">QueryString</span></span>
- <span data-ttu-id="9384a-143">RouteData</span><span class="sxs-lookup"><span data-stu-id="9384a-143">RouteData</span></span>
- <span data-ttu-id="9384a-144">Session</span><span class="sxs-lookup"><span data-stu-id="9384a-144">Session</span></span>
- <span data-ttu-id="9384a-145">UserProfile</span><span class="sxs-lookup"><span data-stu-id="9384a-145">UserProfile</span></span>
- <span data-ttu-id="9384a-146">ViewState</span><span class="sxs-lookup"><span data-stu-id="9384a-146">ViewState</span></span>

<span data-ttu-id="9384a-147">Neste tutorial, você usará o valor de um controle para filtrar quais registros são exibidos no GridView.</span><span class="sxs-lookup"><span data-stu-id="9384a-147">In this tutorial, you will use a control's value to filter which records are displayed in the GridView.</span></span> <span data-ttu-id="9384a-148">Você adicionará o atributo de **controle** ao método de consulta criado anteriormente.</span><span class="sxs-lookup"><span data-stu-id="9384a-148">You will add the **Control** attribute to the query method you had created earlier.</span></span> <span data-ttu-id="9384a-149">Em um tutorial [posterior](using-query-string-values-to-retrieve-data.md) , você aplicará o atributo **QueryString** a um parâmetro para especificar que o valor do parâmetro é proveniente de um valor de cadeia de caracteres de consulta.</span><span class="sxs-lookup"><span data-stu-id="9384a-149">In a [later](using-query-string-values-to-retrieve-data.md) tutorial, you will apply the **QueryString** attribute to a parameter to specify that the parameter value comes from a query string value.</span></span>

<span data-ttu-id="9384a-150">Primeiro, acima do ValidationSummary, adicione uma lista suspensa para filtrar quais alunos são mostrados.</span><span class="sxs-lookup"><span data-stu-id="9384a-150">First, above the ValidationSummary, add a drop down list for filtering which students are shown.</span></span>

[!code-aspx[Main](sorting-paging-and-filtering-data/samples/sample3.aspx?highlight=3-11)]

<span data-ttu-id="9384a-151">No arquivo code-behind, modifique o método Select para receber um valor do controle e defina o nome do parâmetro como o nome do controle que fornece o valor.</span><span class="sxs-lookup"><span data-stu-id="9384a-151">In the code-behind file, modify the select method to receive a value from the control, and set the name of the parameter to the name of the control that provides the value.</span></span>

<span data-ttu-id="9384a-152">Você deve adicionar uma instrução **using** para o namespace **System. Web. modelbinding** para resolver o atributo de controle.</span><span class="sxs-lookup"><span data-stu-id="9384a-152">You must add a **using** statement for the **System.Web.ModelBinding** namespace to resolve the Control attribute.</span></span>

[!code-csharp[Main](sorting-paging-and-filtering-data/samples/sample4.cs)]

<span data-ttu-id="9384a-153">O código a seguir mostra o método Select trabalhado novamente para filtrar os dados retornados com base no valor da lista suspensa.</span><span class="sxs-lookup"><span data-stu-id="9384a-153">The following code shows the select method re-worked to filter the returned data based on the value of the drop down list.</span></span> <span data-ttu-id="9384a-154">A adição de um atributo de controle antes de um parâmetro especifica que o valor desse parâmetro é proveniente de um controle com o mesmo nome.</span><span class="sxs-lookup"><span data-stu-id="9384a-154">Adding a control attribute before a parameter specifies that the value for this parameter comes from a control with the same name.</span></span>

[!code-csharp[Main](sorting-paging-and-filtering-data/samples/sample5.cs)]

<span data-ttu-id="9384a-155">Execute o aplicativo Web e selecione valores diferentes na lista suspensa para filtrar a lista de alunos.</span><span class="sxs-lookup"><span data-stu-id="9384a-155">Run the web application and select different values from the drop down list to filter the list of students.</span></span>

![filtrar alunos](sorting-paging-and-filtering-data/_static/image6.png)

## <a name="conclusion"></a><span data-ttu-id="9384a-157">Conclusão</span><span class="sxs-lookup"><span data-stu-id="9384a-157">Conclusion</span></span>

<span data-ttu-id="9384a-158">Neste tutorial, você habilitou a classificação e a paginação dos dados.</span><span class="sxs-lookup"><span data-stu-id="9384a-158">In this tutorial, you enabled sorting and paging of the data.</span></span> <span data-ttu-id="9384a-159">Você também habilitou a filtragem dos dados pelo valor de um controle.</span><span class="sxs-lookup"><span data-stu-id="9384a-159">You also enabled filtering the data by the value of a control.</span></span>

<span data-ttu-id="9384a-160">No próximo [tutorial](integrating-jquery-ui.md) , você aprimorará a interface do usuário integrando um widget da interface do usuário do jQuery ao modelo de dados dinâmicos.</span><span class="sxs-lookup"><span data-stu-id="9384a-160">In the next [tutorial](integrating-jquery-ui.md) you will enhance the UI by integrating a JQuery UI widget into the dynamic data template.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="9384a-161">[Anterior](updating-deleting-and-creating-data.md)
> [Próximo](integrating-jquery-ui.md)</span><span class="sxs-lookup"><span data-stu-id="9384a-161">[Previous](updating-deleting-and-creating-data.md)
[Next](integrating-jquery-ui.md)</span></span>
