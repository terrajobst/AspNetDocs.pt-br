---
uid: web-forms/overview/presenting-and-managing-data/model-binding/using-query-string-values-to-retrieve-data
title: Usando valores de cadeia de caracteres de consulta para filtrar dados com associação de modelo e formulários da Web | Microsoft Docs
author: Rick-Anderson
description: Esta série de tutoriais demonstra os aspectos básicos do uso de associação de modelo com um projeto ASP.NET Web Forms. A associação de modelo torna a interação de dados mais direta-...
ms.author: riande
ms.date: 02/27/2014
ms.assetid: b90978bd-795d-4871-9ade-1671caff5730
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/using-query-string-values-to-retrieve-data
msc.type: authoredcontent
ms.openlocfilehash: 143ddcb40b576a3129e659b90bfc8321c061a547
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78639093"
---
# <a name="using-query-string-values-to-filter-data-with-model-binding-and-web-forms"></a><span data-ttu-id="1c58a-104">Usando valores de cadeia de caracteres de consulta para filtrar dados com associação de modelo e formulários da Web</span><span class="sxs-lookup"><span data-stu-id="1c58a-104">Using query string values to filter data with model binding and web forms</span></span>

<span data-ttu-id="1c58a-105">por [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="1c58a-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="1c58a-106">Esta série de tutoriais demonstra os aspectos básicos do uso de associação de modelo com um projeto ASP.NET Web Forms.</span><span class="sxs-lookup"><span data-stu-id="1c58a-106">This tutorial series demonstrates basic aspects of using model binding with an ASP.NET Web Forms project.</span></span> <span data-ttu-id="1c58a-107">A associação de modelo torna a interação de dados mais direta do que lidar com objetos de fonte de dados (como ObjectDataSource ou SqlDataSource).</span><span class="sxs-lookup"><span data-stu-id="1c58a-107">Model binding makes data interaction more straight-forward than dealing with data source objects (such as ObjectDataSource or SqlDataSource).</span></span> <span data-ttu-id="1c58a-108">Esta série começa com material introdutório e passa para conceitos mais avançados em Tutoriais posteriores.</span><span class="sxs-lookup"><span data-stu-id="1c58a-108">This series starts with introductory material and moves to more advanced concepts in later tutorials.</span></span>
> 
> <span data-ttu-id="1c58a-109">Este tutorial mostra como passar um valor na cadeia de caracteres de consulta e usar esse valor para recuperar dados por meio de associação de modelo.</span><span class="sxs-lookup"><span data-stu-id="1c58a-109">This tutorial shows how to pass a value in the query string and use that value to retrieve data through model binding.</span></span>
> 
> <span data-ttu-id="1c58a-110">Este tutorial se baseia no projeto criado nas partes [anteriores](retrieving-data.md) da série.</span><span class="sxs-lookup"><span data-stu-id="1c58a-110">This tutorial builds on the project created in the [earlier](retrieving-data.md) parts of the series.</span></span>
> 
> <span data-ttu-id="1c58a-111">Você pode [baixar](https://go.microsoft.com/fwlink/?LinkId=286116) o projeto completo no C# ou no VB.</span><span class="sxs-lookup"><span data-stu-id="1c58a-111">You can [download](https://go.microsoft.com/fwlink/?LinkId=286116) the complete project in C# or VB.</span></span> <span data-ttu-id="1c58a-112">O código para download funciona com o Visual Studio 2012 ou Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="1c58a-112">The downloadable code works with either Visual Studio 2012 or Visual Studio 2013.</span></span> <span data-ttu-id="1c58a-113">Ele usa o modelo do Visual Studio 2012, que é um pouco diferente do modelo de Visual Studio 2013 mostrado neste tutorial.</span><span class="sxs-lookup"><span data-stu-id="1c58a-113">It uses the Visual Studio 2012 template, which is slightly different than the Visual Studio 2013 template shown in this tutorial.</span></span>

## <a name="what-youll-build"></a><span data-ttu-id="1c58a-114">O que você criará</span><span class="sxs-lookup"><span data-stu-id="1c58a-114">What you'll build</span></span>

<span data-ttu-id="1c58a-115">Neste tutorial, você aprenderá a:</span><span class="sxs-lookup"><span data-stu-id="1c58a-115">In this tutorial, you'll:</span></span>

1. <span data-ttu-id="1c58a-116">Adicionar uma nova página para mostrar os cursos registrados para um aluno</span><span class="sxs-lookup"><span data-stu-id="1c58a-116">Add a new page to show the enrolled courses for a student</span></span>
2. <span data-ttu-id="1c58a-117">Recuperar os cursos registrados para o estudante selecionado com base em um valor na cadeia de caracteres de consulta</span><span class="sxs-lookup"><span data-stu-id="1c58a-117">Retrieve the enrolled courses for the selected student based on a value in the query string</span></span>
3. <span data-ttu-id="1c58a-118">Adicionar um hiperlink com um valor de cadeia de caracteres de consulta do modo de exibição de grade para a nova página</span><span class="sxs-lookup"><span data-stu-id="1c58a-118">Add a hyperlink with a query string value from the grid view to the new page</span></span>

<span data-ttu-id="1c58a-119">As etapas neste tutorial são bastante semelhantes às que você fez no [tutorial](sorting-paging-and-filtering-data.md) anterior para filtrar os alunos exibidos com base na seleção de usuário em uma lista suspensa.</span><span class="sxs-lookup"><span data-stu-id="1c58a-119">The steps in this tutorial are fairly similar to what you did in the earlier [tutorial](sorting-paging-and-filtering-data.md) to filter the displayed students based on the user selection in a drop down list.</span></span> <span data-ttu-id="1c58a-120">Nesse tutorial, você usou o atributo **Control** no método Select para especificar que o valor do parâmetro vem de um controle.</span><span class="sxs-lookup"><span data-stu-id="1c58a-120">In that tutorial, you used the **Control** attribute in the select method to specify that the parameter value comes from a control.</span></span> <span data-ttu-id="1c58a-121">Neste tutorial, você usará o atributo **QueryString** no método Select para especificar que o valor do parâmetro é proveniente da cadeia de caracteres de consulta.</span><span class="sxs-lookup"><span data-stu-id="1c58a-121">In this tutorial, you'll use the **QueryString** attribute in the select method to specify that the parameter value comes from the query string.</span></span>

## <a name="add-new-page-for-displaying-a-students-courses"></a><span data-ttu-id="1c58a-122">Adicionar nova página para exibir os cursos de um aluno</span><span class="sxs-lookup"><span data-stu-id="1c58a-122">Add new page for displaying a student's courses</span></span>

<span data-ttu-id="1c58a-123">Adicione um novo formulário da Web que use a página mestra site. Master e nomeie a página **cursos**.</span><span class="sxs-lookup"><span data-stu-id="1c58a-123">Add a new web form that uses the Site.master master page, and name the page **Courses**.</span></span>

<span data-ttu-id="1c58a-124">No arquivo **courses. aspx** , adicione um modo de exibição de grade para exibir os cursos para o aluno selecionado.</span><span class="sxs-lookup"><span data-stu-id="1c58a-124">In the **Courses.aspx** file, add a grid view to display the courses for the selected student.</span></span>

[!code-aspx[Main](using-query-string-values-to-retrieve-data/samples/sample1.aspx)]

## <a name="define-the-select-method"></a><span data-ttu-id="1c58a-125">Definir o método Select</span><span class="sxs-lookup"><span data-stu-id="1c58a-125">Define the select method</span></span>

<span data-ttu-id="1c58a-126">Em **courses.aspx.cs**, você adicionará o método Select com o nome especificado na propriedade **SelectMethod** da exibição de grade.</span><span class="sxs-lookup"><span data-stu-id="1c58a-126">In **Courses.aspx.cs**, you will add the select method with the name you specified in the grid view's **SelectMethod** property.</span></span> <span data-ttu-id="1c58a-127">Nesse método, você definirá a consulta para recuperar os cursos de um aluno e especificará que o parâmetro é proveniente de um valor de cadeia de caracteres de consulta com o mesmo nome que o parâmetro.</span><span class="sxs-lookup"><span data-stu-id="1c58a-127">In that method, you'll define the query for retrieving a student's courses, and specify that the parameter comes from a query string value with the same name as the parameter.</span></span>

<span data-ttu-id="1c58a-128">Primeiro, você deve adicionar as instruções **using** a seguir.</span><span class="sxs-lookup"><span data-stu-id="1c58a-128">First, you must add the following **using** statements.</span></span>

[!code-csharp[Main](using-query-string-values-to-retrieve-data/samples/sample2.cs)]

<span data-ttu-id="1c58a-129">Em seguida, adicione o seguinte código a Courses.aspx.cs:</span><span class="sxs-lookup"><span data-stu-id="1c58a-129">Then, add the following code to Courses.aspx.cs:</span></span>

[!code-csharp[Main](using-query-string-values-to-retrieve-data/samples/sample3.cs)]

<span data-ttu-id="1c58a-130">O atributo QueryString significa que um valor de cadeia de caracteres de consulta chamado StudentId é atribuído automaticamente ao parâmetro nesse método.</span><span class="sxs-lookup"><span data-stu-id="1c58a-130">The QueryString attribute means that a query string value named StudentID is automatically assigned to the parameter in this method.</span></span>

## <a name="add-hyperlink-with-query-string-value"></a><span data-ttu-id="1c58a-131">Adicionar hiperlink com o valor da cadeia de caracteres de consulta</span><span class="sxs-lookup"><span data-stu-id="1c58a-131">Add hyperlink with query string value</span></span>

<span data-ttu-id="1c58a-132">No modo de exibição de grade em students. aspx, você adicionará um campo de hiperlink vinculado à sua nova página de cursos.</span><span class="sxs-lookup"><span data-stu-id="1c58a-132">In the grid view on Students.aspx, you will add a hyperlink field that links to your new Courses page.</span></span> <span data-ttu-id="1c58a-133">O hiperlink incluirá um valor de cadeia de caracteres de consulta com a ID do aluno.</span><span class="sxs-lookup"><span data-stu-id="1c58a-133">The hyperlink will include a query string value with the student's id.</span></span>

<span data-ttu-id="1c58a-134">Em students. aspx, adicione o campo a seguir às colunas de exibição de grade logo abaixo do campo para créditos totais.</span><span class="sxs-lookup"><span data-stu-id="1c58a-134">In Students.aspx, add the following field to the grid view columns just below the field for Total Credits.</span></span>

[!code-aspx[Main](using-query-string-values-to-retrieve-data/samples/sample4.aspx?highlight=7-8)]

<span data-ttu-id="1c58a-135">Execute o aplicativo e observe que o modo de exibição de grade agora inclui o link cursos.</span><span class="sxs-lookup"><span data-stu-id="1c58a-135">Run the application and notice that the grid view now includes the Courses link.</span></span>

![Adicionar hiperlink](using-query-string-values-to-retrieve-data/_static/image1.png)

<span data-ttu-id="1c58a-137">Ao clicar em um dos links, você verá os cursos registrados do aluno.</span><span class="sxs-lookup"><span data-stu-id="1c58a-137">When you click one of the links, you'll see that student's enrolled courses.</span></span>

![Mostrar cursos](using-query-string-values-to-retrieve-data/_static/image2.png)

## <a name="conclusion"></a><span data-ttu-id="1c58a-139">Conclusão</span><span class="sxs-lookup"><span data-stu-id="1c58a-139">Conclusion</span></span>

<span data-ttu-id="1c58a-140">Neste tutorial, você adicionou um link com um valor de cadeia de caracteres de consulta.</span><span class="sxs-lookup"><span data-stu-id="1c58a-140">In this tutorial, you added a link with a query string value.</span></span> <span data-ttu-id="1c58a-141">Você usou esse valor de cadeia de caracteres de consulta para o valor do parâmetro no método Select.</span><span class="sxs-lookup"><span data-stu-id="1c58a-141">You used that query string value for the parameter value in the select method.</span></span>

<span data-ttu-id="1c58a-142">No próximo [tutorial](adding-business-logic-layer.md), você moverá o código dos arquivos code-behind para uma camada de lógica de negócios e uma camada de acesso a dados.</span><span class="sxs-lookup"><span data-stu-id="1c58a-142">In the next [tutorial](adding-business-logic-layer.md), you will move the code from the code-behind files into a business logic layer and a data access layer.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="1c58a-143">[Anterior](integrating-jquery-ui.md)
> [Próximo](adding-business-logic-layer.md)</span><span class="sxs-lookup"><span data-stu-id="1c58a-143">[Previous](integrating-jquery-ui.md)
[Next](adding-business-logic-layer.md)</span></span>
