---
uid: web-forms/overview/presenting-and-managing-data/model-binding/integrating-jquery-ui
title: Integrando a interface do usuário do JQuery DatePicker com associação de modelo e formulários da Web | Microsoft Docs
author: Rick-Anderson
description: Esta série de tutoriais demonstra os aspectos básicos do uso de associação de modelo com um projeto ASP.NET Web Forms. A associação de modelo torna a interação de dados mais direta-...
ms.author: riande
ms.date: 02/27/2014
ms.assetid: 3cbab37b-fb0f-4751-9ec4-74e068c3f380
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/integrating-jquery-ui
msc.type: authoredcontent
ms.openlocfilehash: c8d711dd44950116f3a3e09d5d12c507918c543f
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78642474"
---
# <a name="integrating-jquery-ui-datepicker-with-model-binding-and-web-forms"></a><span data-ttu-id="e7bac-104">Integrando a interface do usuário do JQuery DatePicker com associação de modelo e formulários da Web</span><span class="sxs-lookup"><span data-stu-id="e7bac-104">Integrating JQuery UI Datepicker with model binding and web forms</span></span>

<span data-ttu-id="e7bac-105">por [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="e7bac-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="e7bac-106">Esta série de tutoriais demonstra os aspectos básicos do uso de associação de modelo com um projeto ASP.NET Web Forms.</span><span class="sxs-lookup"><span data-stu-id="e7bac-106">This tutorial series demonstrates basic aspects of using model binding with an ASP.NET Web Forms project.</span></span> <span data-ttu-id="e7bac-107">A associação de modelo torna a interação de dados mais direta do que lidar com objetos de fonte de dados (como ObjectDataSource ou SqlDataSource).</span><span class="sxs-lookup"><span data-stu-id="e7bac-107">Model binding makes data interaction more straight-forward than dealing with data source objects (such as ObjectDataSource or SqlDataSource).</span></span> <span data-ttu-id="e7bac-108">Esta série começa com material introdutório e passa para conceitos mais avançados em Tutoriais posteriores.</span><span class="sxs-lookup"><span data-stu-id="e7bac-108">This series starts with introductory material and moves to more advanced concepts in later tutorials.</span></span>
> 
> <span data-ttu-id="e7bac-109">Este tutorial mostra como adicionar o [widget DatePicker](http://jqueryui.com/datepicker/) da interface do usuário do jQuery a um formulário da Web e usar a associação de modelo para atualizar o banco de dados com o valor selecionado.</span><span class="sxs-lookup"><span data-stu-id="e7bac-109">This tutorial shows how to add the JQuery UI [Datepicker widget](http://jqueryui.com/datepicker/) to a Web Form, and use model binding to update the database with the selected value.</span></span>
> 
> <span data-ttu-id="e7bac-110">Este tutorial se baseia no projeto criado na [primeira](retrieving-data.md) e na [segunda](updating-deleting-and-creating-data.md) parte da série.</span><span class="sxs-lookup"><span data-stu-id="e7bac-110">This tutorial builds on the project created in the [first](retrieving-data.md) and [second](updating-deleting-and-creating-data.md) parts of the series.</span></span>
> 
> <span data-ttu-id="e7bac-111">Você pode [baixar](https://go.microsoft.com/fwlink/?LinkId=286116) o projeto completo no C# ou no VB.</span><span class="sxs-lookup"><span data-stu-id="e7bac-111">You can [download](https://go.microsoft.com/fwlink/?LinkId=286116) the complete project in C# or VB.</span></span> <span data-ttu-id="e7bac-112">O código para download funciona com o Visual Studio 2012 ou Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="e7bac-112">The downloadable code works with either Visual Studio 2012 or Visual Studio 2013.</span></span> <span data-ttu-id="e7bac-113">Ele usa o modelo do Visual Studio 2012, que é um pouco diferente do modelo de Visual Studio 2013 mostrado neste tutorial.</span><span class="sxs-lookup"><span data-stu-id="e7bac-113">It uses the Visual Studio 2012 template, which is slightly different than the Visual Studio 2013 template shown in this tutorial.</span></span>

## <a name="what-youll-build"></a><span data-ttu-id="e7bac-114">O que você criará</span><span class="sxs-lookup"><span data-stu-id="e7bac-114">What you'll build</span></span>

<span data-ttu-id="e7bac-115">Neste tutorial, você aprenderá a:</span><span class="sxs-lookup"><span data-stu-id="e7bac-115">In this tutorial, you'll:</span></span>

1. <span data-ttu-id="e7bac-116">Adicionar uma propriedade ao modelo para registrar a data de registro do aluno</span><span class="sxs-lookup"><span data-stu-id="e7bac-116">Add a property to your model to record the student's enrollment date</span></span>
2. <span data-ttu-id="e7bac-117">Habilitar o usuário a selecionar a data de registro usando o widget DatePicker da interface do usuário do JQuery</span><span class="sxs-lookup"><span data-stu-id="e7bac-117">Enable the user to select the enrollment date using the JQuery UI Datepicker widget</span></span>
3. <span data-ttu-id="e7bac-118">Impor regras de validação para a data de registro</span><span class="sxs-lookup"><span data-stu-id="e7bac-118">Enforce validation rules for the enrollment date</span></span>

<span data-ttu-id="e7bac-119">O widget DatePicker da interface do usuário do JQuery permite que os usuários selecionem facilmente uma data de um calendário que aparece quando o usuário interage com o campo.</span><span class="sxs-lookup"><span data-stu-id="e7bac-119">The JQuery UI Datepicker widget enables users to easily select a date from a calendar that pops up when the user interacts with the field.</span></span> <span data-ttu-id="e7bac-120">O uso desse widget pode ser mais conveniente para usuários do que digitar manualmente uma data.</span><span class="sxs-lookup"><span data-stu-id="e7bac-120">Using this widget can be more convenient for users than manually typing a date.</span></span> <span data-ttu-id="e7bac-121">A integração do widget DatePicker em uma página que usa associação de modelo para operações de dados requer apenas uma pequena quantidade de trabalho adicional.</span><span class="sxs-lookup"><span data-stu-id="e7bac-121">Integrating the Datepicker widget into a page that uses model binding for data operations requires only a small amount of additional work.</span></span>

## <a name="add-a-new-property-to-the-model"></a><span data-ttu-id="e7bac-122">Adicionar uma nova propriedade ao modelo</span><span class="sxs-lookup"><span data-stu-id="e7bac-122">Add a new property to the model</span></span>

<span data-ttu-id="e7bac-123">Primeiro, você adicionará uma propriedade **DateTime** ao modelo de aluno e migrará essa alteração para o banco de dados.</span><span class="sxs-lookup"><span data-stu-id="e7bac-123">First, you will add a **Datetime** property to your Student model and migrate that change to the database.</span></span> <span data-ttu-id="e7bac-124">Abra **UniversityModels.cs**e adicione o código realçado ao modelo de aluno.</span><span class="sxs-lookup"><span data-stu-id="e7bac-124">Open **UniversityModels.cs**, and add the highlighted code to the Student model.</span></span>

[!code-csharp[Main](integrating-jquery-ui/samples/sample1.cs?highlight=16-18)]

<span data-ttu-id="e7bac-125">**RangeAttribute** é incluído para impor regras de validação para a propriedade.</span><span class="sxs-lookup"><span data-stu-id="e7bac-125">The **RangeAttribute** is included to enforce validation rules for the property.</span></span> <span data-ttu-id="e7bac-126">Para este tutorial, vamos pressupor que a Contoso University foi fundada em 1º de janeiro de 2013 e, portanto, as datas de registro anteriores não são válidas.</span><span class="sxs-lookup"><span data-stu-id="e7bac-126">For this tutorial, we will assume that Contoso University was founded on January 1st, 2013 and therefore earlier enrollment dates are not valid.</span></span>

<span data-ttu-id="e7bac-127">Na janela Gerenciamento de Pacotes, adicione uma migração executando o comando **Add-Migration AddEnrollmentDate**.</span><span class="sxs-lookup"><span data-stu-id="e7bac-127">In the Package Management window, add a migration by running the command **add-migration AddEnrollmentDate**.</span></span> <span data-ttu-id="e7bac-128">Observe que o código de migração adiciona a nova coluna datetime à tabela Student.</span><span class="sxs-lookup"><span data-stu-id="e7bac-128">Notice that the migration code adds the new Datetime column to the Student table.</span></span> <span data-ttu-id="e7bac-129">Para corresponder ao valor especificado no RangeAttribute, adicione um valor padrão para a nova coluna, conforme mostrado no código realçado abaixo.</span><span class="sxs-lookup"><span data-stu-id="e7bac-129">To match the value you specified in the RangeAttribute, add a default value for the new column, as shown in the highlighted code below.</span></span>

[!code-csharp[Main](integrating-jquery-ui/samples/sample2.cs?highlight=11)]

<span data-ttu-id="e7bac-130">Salve a alteração no arquivo de migração.</span><span class="sxs-lookup"><span data-stu-id="e7bac-130">Save your change to the migration file.</span></span>

<span data-ttu-id="e7bac-131">Você não precisa propagar os dados novamente.</span><span class="sxs-lookup"><span data-stu-id="e7bac-131">You do not need to seed the data again.</span></span> <span data-ttu-id="e7bac-132">Portanto, abra **Configuration.cs** na pasta migrações e remova ou comente o código no método **semente** .</span><span class="sxs-lookup"><span data-stu-id="e7bac-132">Therefore, open **Configuration.cs** in the Migrations folder and remove or comment out the code in the **Seed** method.</span></span> <span data-ttu-id="e7bac-133">Salve e feche o arquivo.</span><span class="sxs-lookup"><span data-stu-id="e7bac-133">Save and close the file.</span></span>

<span data-ttu-id="e7bac-134">Agora, execute o comando **Update-Database**.</span><span class="sxs-lookup"><span data-stu-id="e7bac-134">Now, run the command **update-database**.</span></span> <span data-ttu-id="e7bac-135">Observe que a coluna agora existe no banco de dados e todos os registros existentes têm o valor padrão para EnrollmentDate.</span><span class="sxs-lookup"><span data-stu-id="e7bac-135">Notice that the column now exists in the database and all of the existing records have the default value for EnrollmentDate.</span></span>

## <a name="add-dynamic-controls-for-enrollment-date"></a><span data-ttu-id="e7bac-136">Adicionar controles dinâmicos para data de registro</span><span class="sxs-lookup"><span data-stu-id="e7bac-136">Add dynamic controls for enrollment date</span></span>

<span data-ttu-id="e7bac-137">Agora, você adicionará controles para exibir e editar a data de registro.</span><span class="sxs-lookup"><span data-stu-id="e7bac-137">You will now add controls for displaying and editing the enrollment date.</span></span> <span data-ttu-id="e7bac-138">Neste ponto, o valor é editado por meio de uma caixa de texto.</span><span class="sxs-lookup"><span data-stu-id="e7bac-138">At this point, the value is edited through a text box.</span></span> <span data-ttu-id="e7bac-139">Posteriormente no tutorial, você alterará a caixa de texto para o widget DatePicker do JQuery.</span><span class="sxs-lookup"><span data-stu-id="e7bac-139">Later in the tutorial, you will change the text box to the JQuery Datepicker widget.</span></span>

<span data-ttu-id="e7bac-140">Primeiro, é importante observar que você não precisa fazer nenhuma alteração no arquivo **addstudent. aspx** .</span><span class="sxs-lookup"><span data-stu-id="e7bac-140">First, it is important to note that you do not need to make any change to the **AddStudent.aspx** file.</span></span> <span data-ttu-id="e7bac-141">O controle DynamicEntity irá renderizar automaticamente a nova propriedade.</span><span class="sxs-lookup"><span data-stu-id="e7bac-141">The DynamicEntity control will automatically render the new property.</span></span>

<span data-ttu-id="e7bac-142">Abra o **students. aspx**e adicione o código realçado a seguir.</span><span class="sxs-lookup"><span data-stu-id="e7bac-142">Open **Students.aspx**, and add the following highlighted code.</span></span>

[!code-aspx[Main](integrating-jquery-ui/samples/sample3.aspx?highlight=13)]

<span data-ttu-id="e7bac-143">Execute o aplicativo e observe que você pode definir o valor da data de registro digitando uma data.</span><span class="sxs-lookup"><span data-stu-id="e7bac-143">Run the application and notice that you can set the value of the enrollment date by typing a date.</span></span> <span data-ttu-id="e7bac-144">Ao adicionar um novo aluno:</span><span class="sxs-lookup"><span data-stu-id="e7bac-144">When adding a new student:</span></span>

![Definir data](integrating-jquery-ui/_static/image1.png)

<span data-ttu-id="e7bac-146">Ou editando um valor existente:</span><span class="sxs-lookup"><span data-stu-id="e7bac-146">Or, editing an existing value:</span></span>

![editar data](integrating-jquery-ui/_static/image2.png)

<span data-ttu-id="e7bac-148">Digitar a data funciona, mas pode não ser a experiência do cliente que você deseja fornecer.</span><span class="sxs-lookup"><span data-stu-id="e7bac-148">Typing the date works, but it might not be the customer experience you wish to provide.</span></span> <span data-ttu-id="e7bac-149">Na próxima seção, você permitirá selecionar uma data por meio de um calendário.</span><span class="sxs-lookup"><span data-stu-id="e7bac-149">In the next section, you will enable selecting a date through a calendar.</span></span>

## <a name="install-nuget-package-to-work-with-jquery-ui"></a><span data-ttu-id="e7bac-150">Instalar o pacote NuGet para trabalhar com a interface do usuário do JQuery</span><span class="sxs-lookup"><span data-stu-id="e7bac-150">Install NuGet package to work with JQuery UI</span></span>

<span data-ttu-id="e7bac-151">O pacote NuGet **da interface do usuário do suco** permite uma fácil integração dos widgets da interface do usuário do jQuery em seu aplicativo Web.</span><span class="sxs-lookup"><span data-stu-id="e7bac-151">The **Juice UI** NuGet package enables easy integration of the JQuery UI widgets into your web application.</span></span> <span data-ttu-id="e7bac-152">Para usar este pacote, instale-o por meio do NuGet.</span><span class="sxs-lookup"><span data-stu-id="e7bac-152">To use this package, install it through NuGet.</span></span>

![Adicionar interface do usuário do suco](integrating-jquery-ui/_static/image3.png)

<span data-ttu-id="e7bac-154">A versão da interface do usuário do suco que você instala pode entrar em conflito com a versão do JQuery em seu aplicativo.</span><span class="sxs-lookup"><span data-stu-id="e7bac-154">The version of Juice UI that you install may conflict with the version of JQuery in your application.</span></span> <span data-ttu-id="e7bac-155">Antes de prosseguir com este tutorial, tente executar o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="e7bac-155">Before proceeding with this tutorial, try running your application.</span></span> <span data-ttu-id="e7bac-156">Se você encontrar um erro de JavaScript, precisará reconciliar a versão do JQuery.</span><span class="sxs-lookup"><span data-stu-id="e7bac-156">If you encounter a JavaScript error, you need to reconcile the JQuery version.</span></span> <span data-ttu-id="e7bac-157">Você pode adicionar a versão esperada do JQuery à sua pasta de scripts (versão 1.8.2 no momento de escrever este tutorial) ou em site. Master especifique o caminho para o arquivo JQuery.</span><span class="sxs-lookup"><span data-stu-id="e7bac-157">You can either add the expected version of JQuery to your Scripts folder (version 1.8.2 at time of writing this tutorial), or in Site.master specify the path to the JQuery file.</span></span>

[!code-aspx[Main](integrating-jquery-ui/samples/sample4.aspx)]

## <a name="customize-datetime-template-to-include-datepicker-widget"></a><span data-ttu-id="e7bac-158">Personalizar o modelo DateTime para incluir o widget DatePicker</span><span class="sxs-lookup"><span data-stu-id="e7bac-158">Customize DateTime template to include Datepicker widget</span></span>

<span data-ttu-id="e7bac-159">Você adicionará o widget DatePicker ao modelo de dados dinâmico para editar um valor DateTime.</span><span class="sxs-lookup"><span data-stu-id="e7bac-159">You will add the Datepicker widget to the dynamic data template for editing a datetime value.</span></span> <span data-ttu-id="e7bac-160">Ao adicionar o widget ao modelo, ele é automaticamente renderizado no formulário para adicionar um novo aluno e no modo de exibição de grade para edição de alunos.</span><span class="sxs-lookup"><span data-stu-id="e7bac-160">By adding the widget to the template, it is automatically rendered in both the form for adding a new student and in the grid view for editing students.</span></span> <span data-ttu-id="e7bac-161">Abra **DateTime\_Edit. ascx**e adicione o seguinte código realçado.</span><span class="sxs-lookup"><span data-stu-id="e7bac-161">Open **DateTime\_Edit.ascx**, and add the following highlighted code.</span></span>

[!code-aspx[Main](integrating-jquery-ui/samples/sample5.aspx?highlight=3)]

<span data-ttu-id="e7bac-162">No arquivo code-behind, você definirá as datas mínima e máxima para o DatePicker.</span><span class="sxs-lookup"><span data-stu-id="e7bac-162">In the code-behind file, you will set the minimum and maximum dates for the DatePicker.</span></span> <span data-ttu-id="e7bac-163">Ao definir esses valores, você impedirá que os usuários naveguem até datas inválidas.</span><span class="sxs-lookup"><span data-stu-id="e7bac-163">By setting these values, you will prevent users from navigating to invalid dates.</span></span> <span data-ttu-id="e7bac-164">Você recuperará os valores mínimo e máximo do **intervaloattribute** na propriedade DateTime, se um for fornecido.</span><span class="sxs-lookup"><span data-stu-id="e7bac-164">You will retrieve the minimum and maximum values from the **RangeAttribute** on the DateTime property, if one is provided.</span></span> <span data-ttu-id="e7bac-165">Abra **DateTime\_Edit.ascx.cs**e adicione o seguinte código realçado à página\_método de carregamento.</span><span class="sxs-lookup"><span data-stu-id="e7bac-165">Open **DateTime\_Edit.ascx.cs**, and add the following highlighted code to the Page\_Load method.</span></span>

[!code-csharp[Main](integrating-jquery-ui/samples/sample6.cs?highlight=9-14)]

<span data-ttu-id="e7bac-166">Execute o aplicativo Web e navegue até a página addaluno.</span><span class="sxs-lookup"><span data-stu-id="e7bac-166">Run the web application and navigate to the AddStudent page.</span></span> <span data-ttu-id="e7bac-167">Forneça valores para os campos e observe que, quando você clica na caixa de texto para data de inscrição, o calendário é exibido.</span><span class="sxs-lookup"><span data-stu-id="e7bac-167">Provide values for the fields and notice that when you click on the text box for Enrollment Date, the calendar is displayed.</span></span>

![seletor de data](integrating-jquery-ui/_static/image4.png)

<span data-ttu-id="e7bac-169">Escolha uma data e clique em **Inserir**.</span><span class="sxs-lookup"><span data-stu-id="e7bac-169">Pick a date, and click **Insert**.</span></span> <span data-ttu-id="e7bac-170">O RangeAttribute impõe a validação no servidor.</span><span class="sxs-lookup"><span data-stu-id="e7bac-170">The RangeAttribute enforces validation on the server.</span></span> <span data-ttu-id="e7bac-171">Ao definir a propriedade mental no DatePicker, você também aplicará a validação no cliente.</span><span class="sxs-lookup"><span data-stu-id="e7bac-171">By setting the minDate property on the Datepicker, you also apply validation on the client.</span></span> <span data-ttu-id="e7bac-172">O calendário não permite que o usuário navegue até uma data anterior ao valor de mente.</span><span class="sxs-lookup"><span data-stu-id="e7bac-172">The calendar does not let the user navigate to a date prior to the value of minDate.</span></span>

<span data-ttu-id="e7bac-173">Quando você edita um registro no modo de exibição de grade, o calendário também é exibido.</span><span class="sxs-lookup"><span data-stu-id="e7bac-173">When you edit a record in the grid view, the calendar is also displayed.</span></span>

![DatePicker em GridView](integrating-jquery-ui/_static/image5.png)

## <a name="conclusion"></a><span data-ttu-id="e7bac-175">Conclusão</span><span class="sxs-lookup"><span data-stu-id="e7bac-175">Conclusion</span></span>

<span data-ttu-id="e7bac-176">Neste tutorial, você aprendeu a incorporar um widget do JQuery em um formulário da Web que usa a associação de modelo.</span><span class="sxs-lookup"><span data-stu-id="e7bac-176">In this tutorial, you learned how to incorporate a JQuery widget into a web form that uses model binding.</span></span>

<span data-ttu-id="e7bac-177">No próximo [tutorial](using-query-string-values-to-retrieve-data.md), você usará um valor de cadeia de caracteres de consulta ao selecionar dados.</span><span class="sxs-lookup"><span data-stu-id="e7bac-177">In the next [tutorial](using-query-string-values-to-retrieve-data.md), you will use a query string value when selecting data.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="e7bac-178">[Anterior](sorting-paging-and-filtering-data.md)
> [Próximo](using-query-string-values-to-retrieve-data.md)</span><span class="sxs-lookup"><span data-stu-id="e7bac-178">[Previous](sorting-paging-and-filtering-data.md)
[Next](using-query-string-values-to-retrieve-data.md)</span></span>
