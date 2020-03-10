---
uid: web-forms/overview/presenting-and-managing-data/model-binding/updating-deleting-and-creating-data
title: Atualizando, excluindo e criando dados com associação de modelo e formulários da Web | Microsoft Docs
author: Rick-Anderson
description: Esta série de tutoriais demonstra os aspectos básicos do uso de associação de modelo com um projeto ASP.NET Web Forms. A associação de modelo torna a interação de dados mais direta-...
ms.author: riande
ms.date: 02/27/2014
ms.assetid: 602baa94-5a4f-46eb-a717-7a9e539c1db4
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/updating-deleting-and-creating-data
msc.type: authoredcontent
ms.openlocfilehash: 11dc52ec79ca91119b37ea60e08164eb1b1d0e2b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78586642"
---
# <a name="updating-deleting-and-creating-data-with-model-binding-and-web-forms"></a><span data-ttu-id="fcdd0-104">Atualizando, excluindo e criando dados com associação de modelo e formulários da Web</span><span class="sxs-lookup"><span data-stu-id="fcdd0-104">Updating, deleting, and creating data with model binding and web forms</span></span>

<span data-ttu-id="fcdd0-105">por [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="fcdd0-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="fcdd0-106">Esta série de tutoriais demonstra os aspectos básicos do uso de associação de modelo com um projeto ASP.NET Web Forms.</span><span class="sxs-lookup"><span data-stu-id="fcdd0-106">This tutorial series demonstrates basic aspects of using model binding with an ASP.NET Web Forms project.</span></span> <span data-ttu-id="fcdd0-107">A associação de modelo torna a interação de dados mais direta do que lidar com objetos de fonte de dados (como ObjectDataSource ou SqlDataSource).</span><span class="sxs-lookup"><span data-stu-id="fcdd0-107">Model binding makes data interaction more straight-forward than dealing with data source objects (such as ObjectDataSource or SqlDataSource).</span></span> <span data-ttu-id="fcdd0-108">Esta série começa com material introdutório e passa para conceitos mais avançados em Tutoriais posteriores.</span><span class="sxs-lookup"><span data-stu-id="fcdd0-108">This series starts with introductory material and moves to more advanced concepts in later tutorials.</span></span>
> 
> <span data-ttu-id="fcdd0-109">Este tutorial mostra como criar, atualizar e excluir dados com associação de modelo.</span><span class="sxs-lookup"><span data-stu-id="fcdd0-109">This tutorial shows how to create, update, and delete data with model binding.</span></span> <span data-ttu-id="fcdd0-110">Você definirá as seguintes propriedades:</span><span class="sxs-lookup"><span data-stu-id="fcdd0-110">You will set the following properties:</span></span>
> 
> - <span data-ttu-id="fcdd0-111">DeleteMethod</span><span class="sxs-lookup"><span data-stu-id="fcdd0-111">DeleteMethod</span></span>
> - <span data-ttu-id="fcdd0-112">InsertMethod</span><span class="sxs-lookup"><span data-stu-id="fcdd0-112">InsertMethod</span></span>
> - <span data-ttu-id="fcdd0-113">UpdateMethod</span><span class="sxs-lookup"><span data-stu-id="fcdd0-113">UpdateMethod</span></span>
> 
> <span data-ttu-id="fcdd0-114">Essas propriedades recebem o nome do método que manipula a operação correspondente.</span><span class="sxs-lookup"><span data-stu-id="fcdd0-114">These properties receive the name of the method that handles the corresponding operation.</span></span> <span data-ttu-id="fcdd0-115">Dentro desse método, você fornece a lógica para interagir com os dados.</span><span class="sxs-lookup"><span data-stu-id="fcdd0-115">Within that method, you provide the logic for interacting with the data.</span></span>
> 
> <span data-ttu-id="fcdd0-116">Este tutorial se baseia no projeto criado na primeira [parte](retrieving-data.md) da série.</span><span class="sxs-lookup"><span data-stu-id="fcdd0-116">This tutorial builds on the project created in the first [part](retrieving-data.md) of the series.</span></span>
> 
> <span data-ttu-id="fcdd0-117">Você pode [baixar](https://go.microsoft.com/fwlink/?LinkId=286116) o projeto completo no C# ou no VB.</span><span class="sxs-lookup"><span data-stu-id="fcdd0-117">You can [download](https://go.microsoft.com/fwlink/?LinkId=286116) the complete project in C# or VB.</span></span> <span data-ttu-id="fcdd0-118">O código para download funciona com o Visual Studio 2012 ou Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="fcdd0-118">The downloadable code works with either Visual Studio 2012 or Visual Studio 2013.</span></span> <span data-ttu-id="fcdd0-119">Ele usa o modelo do Visual Studio 2012, que é um pouco diferente do modelo de Visual Studio 2013 mostrado neste tutorial.</span><span class="sxs-lookup"><span data-stu-id="fcdd0-119">It uses the Visual Studio 2012 template, which is slightly different than the Visual Studio 2013 template shown in this tutorial.</span></span>

## <a name="what-youll-build"></a><span data-ttu-id="fcdd0-120">O que você criará</span><span class="sxs-lookup"><span data-stu-id="fcdd0-120">What you'll build</span></span>

<span data-ttu-id="fcdd0-121">Neste tutorial, você aprenderá a:</span><span class="sxs-lookup"><span data-stu-id="fcdd0-121">In this tutorial, you'll:</span></span>

1. <span data-ttu-id="fcdd0-122">Adicionar modelos de dados dinâmicos</span><span class="sxs-lookup"><span data-stu-id="fcdd0-122">Add dynamic data templates</span></span>
2. <span data-ttu-id="fcdd0-123">Habilitar atualização e exclusão de dados por meio de métodos de associação de modelo</span><span class="sxs-lookup"><span data-stu-id="fcdd0-123">Enable updating and deleting data through model binding methods</span></span>
3. <span data-ttu-id="fcdd0-124">Aplicar regras de validação de dados – habilitar a criação de um novo registro no banco</span><span class="sxs-lookup"><span data-stu-id="fcdd0-124">Apply data validation rules - Enable creating a new record in the database</span></span>

## <a name="add-dynamic-data-templates"></a><span data-ttu-id="fcdd0-125">Adicionar modelos de dados dinâmicos</span><span class="sxs-lookup"><span data-stu-id="fcdd0-125">Add dynamic data templates</span></span>

<span data-ttu-id="fcdd0-126">Para fornecer a melhor experiência de usuário e minimizar a repetição de código, você usará modelos de dados dinâmicos.</span><span class="sxs-lookup"><span data-stu-id="fcdd0-126">To provide the best user experience and minimize code repetition, you will use dynamic data templates.</span></span> <span data-ttu-id="fcdd0-127">Você pode integrar facilmente modelos de dados dinâmicos pré-criados em seu site existente instalando um pacote NuGet.</span><span class="sxs-lookup"><span data-stu-id="fcdd0-127">You can easily integrate pre-built dynamic data templates into your existing site by installing a NuGet package.</span></span>

<span data-ttu-id="fcdd0-128">Em **gerenciar pacotes NuGet**, instale o **DynamicDataTemplatesCS**.</span><span class="sxs-lookup"><span data-stu-id="fcdd0-128">From the **Manage NuGet Packages**, install the **DynamicDataTemplatesCS**.</span></span>

![modelos de dados dinâmicos](updating-deleting-and-creating-data/_static/image1.png)

<span data-ttu-id="fcdd0-130">Observe que o projeto agora inclui uma pasta chamada **DynamicData**.</span><span class="sxs-lookup"><span data-stu-id="fcdd0-130">Notice that your project now includes a folder named **DynamicData**.</span></span> <span data-ttu-id="fcdd0-131">Nessa pasta, você encontrará os modelos que são aplicados automaticamente aos controles dinâmicos em seus Web Forms.</span><span class="sxs-lookup"><span data-stu-id="fcdd0-131">In that folder, you will find the templates that are automatically applied to dynamic controls in your web forms.</span></span>

![pasta de dados dinâmicos](updating-deleting-and-creating-data/_static/image2.png)

## <a name="enable-updating-and-deleting"></a><span data-ttu-id="fcdd0-133">Habilitar atualização e exclusão</span><span class="sxs-lookup"><span data-stu-id="fcdd0-133">Enable updating and deleting</span></span>

<span data-ttu-id="fcdd0-134">Permitir que os usuários atualizem e excluam registros no banco de dados é muito semelhante ao processo de recuperação de dado.</span><span class="sxs-lookup"><span data-stu-id="fcdd0-134">Enabling users to update and delete records in the database is very similar to the process for retrieving data.</span></span> <span data-ttu-id="fcdd0-135">Nas propriedades **UpdateMethod** e **DeleteMethod** , você especifica os nomes dos métodos que executam essas operações.</span><span class="sxs-lookup"><span data-stu-id="fcdd0-135">In the **UpdateMethod** and **DeleteMethod** properties, you specify the names of the methods that perform those operations.</span></span> <span data-ttu-id="fcdd0-136">Com um controle GridView, você também pode especificar a geração automática de botões editar e excluir.</span><span class="sxs-lookup"><span data-stu-id="fcdd0-136">With a GridView control, you can also specify the automatic generation of edit and delete buttons.</span></span> <span data-ttu-id="fcdd0-137">O código realçado a seguir mostra as adições ao código GridView.</span><span class="sxs-lookup"><span data-stu-id="fcdd0-137">The following highlighted code shows the additions to the GridView code.</span></span>

[!code-aspx[Main](updating-deleting-and-creating-data/samples/sample1.aspx?highlight=4-5)]

<span data-ttu-id="fcdd0-138">No arquivo code-behind, adicione uma instrução using para **System. Data. Entity. Infrastructure**.</span><span class="sxs-lookup"><span data-stu-id="fcdd0-138">In the code-behind file, add a using statement for **System.Data.Entity.Infrastructure**.</span></span>

[!code-csharp[Main](updating-deleting-and-creating-data/samples/sample2.cs)]

<span data-ttu-id="fcdd0-139">Em seguida, adicione os seguintes métodos Update e Delete.</span><span class="sxs-lookup"><span data-stu-id="fcdd0-139">Then, add the following update and delete methods.</span></span>

[!code-csharp[Main](updating-deleting-and-creating-data/samples/sample3.cs)]

<span data-ttu-id="fcdd0-140">O método **TryUpdateModel** aplica os valores de associação de dados correspondentes do formulário da Web ao item de dados.</span><span class="sxs-lookup"><span data-stu-id="fcdd0-140">The **TryUpdateModel** method applies the matching data-bound values from the web form to the data item.</span></span> <span data-ttu-id="fcdd0-141">O item de dados é recuperado com base no valor do parâmetro ID.</span><span class="sxs-lookup"><span data-stu-id="fcdd0-141">The data item is retrieved based on the value of the id parameter.</span></span>

## <a name="enforce-validation-requirements"></a><span data-ttu-id="fcdd0-142">Impor requisitos de validação</span><span class="sxs-lookup"><span data-stu-id="fcdd0-142">Enforce validation requirements</span></span>

<span data-ttu-id="fcdd0-143">Os atributos de validação aplicados às propriedades FirstName, LastName e year na classe Student são aplicados automaticamente ao atualizar os dados.</span><span class="sxs-lookup"><span data-stu-id="fcdd0-143">The validation attributes that you applied to the FirstName, LastName, and Year properties in the Student class are automatically enforced when updating the data.</span></span> <span data-ttu-id="fcdd0-144">Os controles DynamicField adicionam validadores de cliente e de servidor com base nos atributos de validação.</span><span class="sxs-lookup"><span data-stu-id="fcdd0-144">The DynamicField controls add client and server validators based on the validation attributes.</span></span> <span data-ttu-id="fcdd0-145">As propriedades FirstName e LastName são obrigatórias.</span><span class="sxs-lookup"><span data-stu-id="fcdd0-145">The FirstName and LastName properties are both required.</span></span> <span data-ttu-id="fcdd0-146">FirstName não pode exceder 20 caracteres de comprimento e LastName não pode exceder 40 caracteres.</span><span class="sxs-lookup"><span data-stu-id="fcdd0-146">FirstName cannot exceed 20 characters in length, and LastName cannot exceed 40 characters.</span></span> <span data-ttu-id="fcdd0-147">Year deve ser um valor válido para a enumeração AcademicYear.</span><span class="sxs-lookup"><span data-stu-id="fcdd0-147">Year must be a valid value for the AcademicYear enumeration.</span></span>

<span data-ttu-id="fcdd0-148">Se o usuário violar um dos requisitos de validação, a atualização não continuará.</span><span class="sxs-lookup"><span data-stu-id="fcdd0-148">If the user violates one of the validation requirements, the update does not proceed.</span></span> <span data-ttu-id="fcdd0-149">Para ver a mensagem de erro, adicione um controle ValidationSummary acima do GridView.</span><span class="sxs-lookup"><span data-stu-id="fcdd0-149">To see the error message, add a ValidationSummary control above the GridView.</span></span> <span data-ttu-id="fcdd0-150">Para exibir os erros de validação da Associação de modelo, defina a propriedade **ShowModelStateErrors** definida como **true**.</span><span class="sxs-lookup"><span data-stu-id="fcdd0-150">To display the validation errors from model binding, set the **ShowModelStateErrors** property set to **true**.</span></span> 

[!code-aspx[Main](updating-deleting-and-creating-data/samples/sample4.aspx)]

<span data-ttu-id="fcdd0-151">Execute o aplicativo Web e atualize e exclua qualquer um dos registros.</span><span class="sxs-lookup"><span data-stu-id="fcdd0-151">Run the web application, and update and delete any of the records.</span></span>

![atualizar dados](updating-deleting-and-creating-data/_static/image3.png)

<span data-ttu-id="fcdd0-153">Observe que, quando estiver no modo de edição, o valor da propriedade year será automaticamente renderizado como uma lista suspensa.</span><span class="sxs-lookup"><span data-stu-id="fcdd0-153">Notice that when in the edit mode the value for the Year property is automatically rendered as a drop down list.</span></span> <span data-ttu-id="fcdd0-154">A propriedade year é um valor de enumeração e o modelo de dados dinâmicos para um valor de enumeração Especifica uma lista suspensa para edição.</span><span class="sxs-lookup"><span data-stu-id="fcdd0-154">The Year property is an enumeration value, and the dynamic data template for an enumeration value specifies a drop down list for editing.</span></span> <span data-ttu-id="fcdd0-155">Você pode encontrar esse modelo abrindo a **enumeração\_arquivo Edit. ascx** na pasta **DynamicData**/**FieldTemplates** .</span><span class="sxs-lookup"><span data-stu-id="fcdd0-155">You can find that template by opening the **Enumeration\_Edit.ascx** file in the **DynamicData**/**FieldTemplates** folder.</span></span>

<span data-ttu-id="fcdd0-156">Se você fornecer valores válidos, a atualização será concluída com êxito.</span><span class="sxs-lookup"><span data-stu-id="fcdd0-156">If you provide valid values, the update completes successfully.</span></span> <span data-ttu-id="fcdd0-157">Se você violar um dos requisitos de validação, a atualização não continuará e uma mensagem de erro será exibida acima da grade.</span><span class="sxs-lookup"><span data-stu-id="fcdd0-157">If you violate one of the validation requirements, the update does not proceed and an error message is displayed above the grid.</span></span>

![mensagem de erro](updating-deleting-and-creating-data/_static/image4.png)

## <a name="add-new-records"></a><span data-ttu-id="fcdd0-159">Adicionar novos registros</span><span class="sxs-lookup"><span data-stu-id="fcdd0-159">Add new records</span></span>

<span data-ttu-id="fcdd0-160">O controle GridView não inclui a propriedade **InsertMethod** e, portanto, não pode ser usado para adicionar um novo registro com associação de modelo.</span><span class="sxs-lookup"><span data-stu-id="fcdd0-160">The GridView control does not include the **InsertMethod** property and therefore cannot be used for adding a new record with model binding.</span></span> <span data-ttu-id="fcdd0-161">Você pode encontrar a propriedade InsertMethod nos controles **FormView**, **DetailsView**ou **ListView** .</span><span class="sxs-lookup"><span data-stu-id="fcdd0-161">You can find the InsertMethod property in the **FormView**, **DetailsView**, or **ListView** controls.</span></span> <span data-ttu-id="fcdd0-162">Neste tutorial, você usará um controle FormView para adicionar um novo registro.</span><span class="sxs-lookup"><span data-stu-id="fcdd0-162">In this tutorial, you will use a FormView control to add a new record.</span></span>

<span data-ttu-id="fcdd0-163">Primeiro, adicione um link para a nova página que será criada para adicionar um novo registro.</span><span class="sxs-lookup"><span data-stu-id="fcdd0-163">First, add a link to the new page you will create for adding a new record.</span></span> <span data-ttu-id="fcdd0-164">Acima do ValidationSummary, adicione:</span><span class="sxs-lookup"><span data-stu-id="fcdd0-164">Above the ValidationSummary, add:</span></span>

[!code-aspx[Main](updating-deleting-and-creating-data/samples/sample5.aspx)]

<span data-ttu-id="fcdd0-165">O novo link aparecerá na parte superior do conteúdo para a página dos alunos.</span><span class="sxs-lookup"><span data-stu-id="fcdd0-165">The new link will appear at the top of the content for the Students page.</span></span>

![novo link](updating-deleting-and-creating-data/_static/image5.png)

<span data-ttu-id="fcdd0-167">Em seguida, adicione um novo formulário da Web usando uma página mestra e nomeie-o como **mystudent**.</span><span class="sxs-lookup"><span data-stu-id="fcdd0-167">Then, add a new web form using a master page, and name it **AddStudent**.</span></span> <span data-ttu-id="fcdd0-168">Selecione site. master como a página mestra.</span><span class="sxs-lookup"><span data-stu-id="fcdd0-168">Select Site.Master as the master page.</span></span>

<span data-ttu-id="fcdd0-169">Você renderizará os campos para adicionar um novo aluno usando um controle **DynamicEntity** .</span><span class="sxs-lookup"><span data-stu-id="fcdd0-169">You will render the fields for adding a new student by using a **DynamicEntity** control.</span></span> <span data-ttu-id="fcdd0-170">O controle DynamicEntity renderiza as propriedades editáveis na classe especificada na Propriedade ItemType.</span><span class="sxs-lookup"><span data-stu-id="fcdd0-170">The DynamicEntity control renders that editable properties in the class specified in the ItemType property.</span></span> <span data-ttu-id="fcdd0-171">A propriedade StudentId foi marcada com o atributo **[ScaffoldColumn (false)]** para que não seja renderizada.</span><span class="sxs-lookup"><span data-stu-id="fcdd0-171">The StudentID property was marked with the **[ScaffoldColumn(false)]** attribute so it is not rendered.</span></span> <span data-ttu-id="fcdd0-172">No espaço reservado MainContent da página addaluno, adicione o código a seguir.</span><span class="sxs-lookup"><span data-stu-id="fcdd0-172">In the MainContent placeholder of the AddStudent page, add the following code.</span></span>

[!code-aspx[Main](updating-deleting-and-creating-data/samples/sample6.aspx)]

<span data-ttu-id="fcdd0-173">No arquivo code-behind (AddStudent.aspx.cs), adicione uma instrução **using** para o namespace **ContosoUniversityModelBinding. Models** .</span><span class="sxs-lookup"><span data-stu-id="fcdd0-173">In the code-behind file (AddStudent.aspx.cs), add a **using** statement for the **ContosoUniversityModelBinding.Models** namespace.</span></span>

[!code-csharp[Main](updating-deleting-and-creating-data/samples/sample7.cs)]

<span data-ttu-id="fcdd0-174">Em seguida, adicione os métodos a seguir para especificar como inserir um novo registro e um manipulador de eventos para o botão Cancelar.</span><span class="sxs-lookup"><span data-stu-id="fcdd0-174">Then, add the following methods to specify how to insert a new record and an event handler for the cancel button.</span></span>

[!code-csharp[Main](updating-deleting-and-creating-data/samples/sample8.cs)]

<span data-ttu-id="fcdd0-175">Salve todas as alterações.</span><span class="sxs-lookup"><span data-stu-id="fcdd0-175">Save all of the changes.</span></span>

<span data-ttu-id="fcdd0-176">Execute o aplicativo Web e crie um novo aluno.</span><span class="sxs-lookup"><span data-stu-id="fcdd0-176">Run the web application and create a new student.</span></span>

![Adicionar novo aluno](updating-deleting-and-creating-data/_static/image6.png)

<span data-ttu-id="fcdd0-178">Clique em **Inserir** e observe que o novo aluno foi criado.</span><span class="sxs-lookup"><span data-stu-id="fcdd0-178">Click **Insert** and notice the new student has been created.</span></span>

![exibir novo aluno](updating-deleting-and-creating-data/_static/image7.png)

## <a name="conclusion"></a><span data-ttu-id="fcdd0-180">Conclusão</span><span class="sxs-lookup"><span data-stu-id="fcdd0-180">Conclusion</span></span>

<span data-ttu-id="fcdd0-181">Neste tutorial, você habilitou a atualização, a exclusão e a criação de dados.</span><span class="sxs-lookup"><span data-stu-id="fcdd0-181">In this tutorial, you enabled updating, deleting, and creating data.</span></span> <span data-ttu-id="fcdd0-182">Você assegurau que as regras de validação são aplicadas ao interagir com os dados.</span><span class="sxs-lookup"><span data-stu-id="fcdd0-182">You ensured validation rules are applied when interacting with the data.</span></span>

<span data-ttu-id="fcdd0-183">No próximo [tutorial](sorting-paging-and-filtering-data.md) desta série, você permitirá classificar, paginar e filtrar dados.</span><span class="sxs-lookup"><span data-stu-id="fcdd0-183">In the next [tutorial](sorting-paging-and-filtering-data.md) in this series, you will enable sorting, paging, and filtering data.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="fcdd0-184">[Anterior](retrieving-data.md)
> [Próximo](sorting-paging-and-filtering-data.md)</span><span class="sxs-lookup"><span data-stu-id="fcdd0-184">[Previous](retrieving-data.md)
[Next](sorting-paging-and-filtering-data.md)</span></span>
