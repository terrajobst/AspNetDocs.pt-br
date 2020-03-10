---
uid: mvc/overview/getting-started/database-first-development/enhancing-data-validation
title: 'Tutorial: aprimorar a validação de dados para o EF Database First com o aplicativo MVC ASP.NET'
description: Este tutorial se concentra na adição de anotações de dados ao modelo de dados para especificar requisitos de validação e formatação de exibição.
author: Rick-Anderson
ms.author: riande
ms.date: 01/28/2019
ms.topic: tutorial
ms.assetid: 0ed5e67a-34c0-4b57-84a6-802b0fb3cd00
msc.legacyurl: /mvc/overview/getting-started/database-first-development/enhancing-data-validation
msc.type: authoredcontent
ms.openlocfilehash: 897cd7c6a40445e2a4abede50d81e101372d3233
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78616273"
---
# <a name="tutorial-enhance-data-validation-for-ef-database-first-with-aspnet-mvc-app"></a><span data-ttu-id="9e070-103">Tutorial: aprimorar a validação de dados para o EF Database First com o aplicativo MVC ASP.NET</span><span class="sxs-lookup"><span data-stu-id="9e070-103">Tutorial: Enhance data validation for EF Database First with ASP.NET MVC app</span></span>

<span data-ttu-id="9e070-104">Usando MVC, Entity Framework e ASP.NET scaffolding, você pode criar um aplicativo Web que fornece uma interface para um banco de dados existente.</span><span class="sxs-lookup"><span data-stu-id="9e070-104">Using MVC, Entity Framework, and ASP.NET Scaffolding, you can create a web application that provides an interface to an existing database.</span></span> <span data-ttu-id="9e070-105">Esta série de tutoriais mostra como gerar automaticamente o código que permite que os usuários exibam, editem, criem e excluam dados que residem em uma tabela.</span><span class="sxs-lookup"><span data-stu-id="9e070-105">This tutorial series shows you how to automatically generate code that enables users to display, edit, create, and delete data that resides in a database table.</span></span> <span data-ttu-id="9e070-106">O código gerado corresponde às colunas na tabela de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="9e070-106">The generated code corresponds to the columns in the database table.</span></span>

<span data-ttu-id="9e070-107">Este tutorial se concentra na adição de anotações de dados ao modelo de dados para especificar requisitos de validação e formatação de exibição.</span><span class="sxs-lookup"><span data-stu-id="9e070-107">This tutorial focuses on adding data annotations to the data model to specify validation requirements and display formatting.</span></span> <span data-ttu-id="9e070-108">Ele foi aprimorado com base nos comentários dos usuários na seção de comentários.</span><span class="sxs-lookup"><span data-stu-id="9e070-108">It was improved based on feedback from users in the comments section.</span></span>

<span data-ttu-id="9e070-109">Neste tutorial, você:</span><span class="sxs-lookup"><span data-stu-id="9e070-109">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="9e070-110">Adicionar anotações de dados</span><span class="sxs-lookup"><span data-stu-id="9e070-110">Add data annotations</span></span>
> * <span data-ttu-id="9e070-111">Adicionar classes de metadados</span><span class="sxs-lookup"><span data-stu-id="9e070-111">Add metadata classes</span></span>

## <a name="prerequisites"></a><span data-ttu-id="9e070-112">Prerequisites</span><span class="sxs-lookup"><span data-stu-id="9e070-112">Prerequisites</span></span>

* [<span data-ttu-id="9e070-113">Personalizar uma exibição</span><span class="sxs-lookup"><span data-stu-id="9e070-113">Customize a view</span></span>](customizing-a-view.md)

## <a name="add-data-annotations"></a><span data-ttu-id="9e070-114">Adicionar anotações de dados</span><span class="sxs-lookup"><span data-stu-id="9e070-114">Add data annotations</span></span>

<span data-ttu-id="9e070-115">Como vimos em um tópico anterior, algumas regras de validação de dados são aplicadas automaticamente à entrada do usuário.</span><span class="sxs-lookup"><span data-stu-id="9e070-115">As you saw in an earlier topic, some data validation rules are automatically applied to the user input.</span></span> <span data-ttu-id="9e070-116">Por exemplo, você só pode fornecer um número para a propriedade grau.</span><span class="sxs-lookup"><span data-stu-id="9e070-116">For example, you can only provide a number for the Grade property.</span></span> <span data-ttu-id="9e070-117">Para especificar mais regras de validação de dados, você pode adicionar anotações de dados à sua classe de modelo.</span><span class="sxs-lookup"><span data-stu-id="9e070-117">To specify more data validation rules, you can add data annotations to your model class.</span></span> <span data-ttu-id="9e070-118">Essas anotações são aplicadas em todo o aplicativo Web para a propriedade especificada.</span><span class="sxs-lookup"><span data-stu-id="9e070-118">These annotations are applied throughout your web application for the specified property.</span></span> <span data-ttu-id="9e070-119">Você também pode aplicar atributos de formatação que alteram como as propriedades são exibidas; como, alterar o valor usado para rótulos de texto.</span><span class="sxs-lookup"><span data-stu-id="9e070-119">You can also apply formatting attributes that change how the properties are displayed; such as, changing the value used for text labels.</span></span>

<span data-ttu-id="9e070-120">Neste tutorial, você adicionará anotações de dados para restringir o comprimento dos valores fornecidos para as propriedades FirstName, LastName e MiddleName.</span><span class="sxs-lookup"><span data-stu-id="9e070-120">In this tutorial, you will add data annotations to restrict the length of the values provided for the FirstName, LastName, and MiddleName properties.</span></span> <span data-ttu-id="9e070-121">No banco de dados, esses valores são limitados a 50 caracteres; no entanto, no aplicativo Web, esse limite de caracteres não é imposto no momento.</span><span class="sxs-lookup"><span data-stu-id="9e070-121">In the database, these values are limited to 50 characters; however, in your web application that character limit is currently not enforced.</span></span> <span data-ttu-id="9e070-122">Se um usuário fornecer mais de 50 caracteres para um desses valores, a página falhará ao tentar salvar o valor no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="9e070-122">If a user provides more than 50 characters for one of those values, the page will crash when attempting to save the value to the database.</span></span> <span data-ttu-id="9e070-123">Você também restringirá a taxa de valores entre 0 e 4.</span><span class="sxs-lookup"><span data-stu-id="9e070-123">You will also restrict Grade to values between 0 and 4.</span></span>

<span data-ttu-id="9e070-124">Selecione **modelos** > **ContosoModel. edmx** > **ContosoModel.tt** e abra o arquivo *Student.cs* .</span><span class="sxs-lookup"><span data-stu-id="9e070-124">Select **Models** > **ContosoModel.edmx** > **ContosoModel.tt** and open the *Student.cs* file.</span></span> <span data-ttu-id="9e070-125">Adicione o seguinte código realçado à classe.</span><span class="sxs-lookup"><span data-stu-id="9e070-125">Add the following highlighted code to the class.</span></span>

[!code-csharp[Main](enhancing-data-validation/samples/sample1.cs?highlight=5,15,17,20)]

<span data-ttu-id="9e070-126">Abra *Enrollment.cs* e adicione o seguinte código realçado.</span><span class="sxs-lookup"><span data-stu-id="9e070-126">Open *Enrollment.cs* and add the following highlighted code.</span></span>

[!code-csharp[Main](enhancing-data-validation/samples/sample2.cs?highlight=5,10)]

<span data-ttu-id="9e070-127">Compile a solução.</span><span class="sxs-lookup"><span data-stu-id="9e070-127">Build the solution.</span></span>

<span data-ttu-id="9e070-128">Clique em **lista de alunos** e selecione **Editar**.</span><span class="sxs-lookup"><span data-stu-id="9e070-128">Click **List of students** and select **Edit**.</span></span> <span data-ttu-id="9e070-129">Se você tentar inserir mais de 50 caracteres, uma mensagem de erro será exibida.</span><span class="sxs-lookup"><span data-stu-id="9e070-129">If you attempt to enter more than 50 characters, an error message is displayed.</span></span>

![Mostrar mensagem de erro](enhancing-data-validation/_static/image1.png)

<span data-ttu-id="9e070-131">Volte para a home page.</span><span class="sxs-lookup"><span data-stu-id="9e070-131">Go back to the home page.</span></span> <span data-ttu-id="9e070-132">Clique em **lista de registros** e selecione **Editar**.</span><span class="sxs-lookup"><span data-stu-id="9e070-132">Click **List of enrollments** and select **Edit**.</span></span> <span data-ttu-id="9e070-133">Tente fornecer uma classificação acima de 4.</span><span class="sxs-lookup"><span data-stu-id="9e070-133">Attempt to provide a grade above 4.</span></span> <span data-ttu-id="9e070-134">Você receberá esse erro: *a classificação do campo deve estar entre 0 e 4.*</span><span class="sxs-lookup"><span data-stu-id="9e070-134">You will receive this error: *The field Grade must be between 0 and 4.*</span></span>

## <a name="add-metadata-classes"></a><span data-ttu-id="9e070-135">Adicionar classes de metadados</span><span class="sxs-lookup"><span data-stu-id="9e070-135">Add metadata classes</span></span>

<span data-ttu-id="9e070-136">Adicionar os atributos de validação diretamente à classe de modelo funciona quando você não espera que o banco de dados seja alterado; no entanto, se o banco de dados for alterado e você precisar regenerar a classe de modelo, perderá todos os atributos que você aplicou à classe de modelo.</span><span class="sxs-lookup"><span data-stu-id="9e070-136">Adding the validation attributes directly to the model class works when you do not expect the database to change; however, if your database changes and you need to regenerate the model class, you will lose all of the attributes you had applied to the model class.</span></span> <span data-ttu-id="9e070-137">Essa abordagem pode ser muito ineficiente e propenso a perder regras de validação importantes.</span><span class="sxs-lookup"><span data-stu-id="9e070-137">This approach can be very inefficient and prone to losing important validation rules.</span></span>

<span data-ttu-id="9e070-138">Para evitar esse problema, você pode adicionar uma classe de metadados que contém os atributos.</span><span class="sxs-lookup"><span data-stu-id="9e070-138">To avoid this problem, you can add a metadata class that contains the attributes.</span></span> <span data-ttu-id="9e070-139">Quando você associa a classe de modelo à classe de metadados, esses atributos são aplicados ao modelo.</span><span class="sxs-lookup"><span data-stu-id="9e070-139">When you associate the model class to the metadata class, those attributes are applied to the model.</span></span> <span data-ttu-id="9e070-140">Nessa abordagem, a classe de modelo pode ser regenerada sem perder todos os atributos que foram aplicados à classe de metadados.</span><span class="sxs-lookup"><span data-stu-id="9e070-140">In this approach, the model class can be regenerated without losing all of the attributes that have been applied to the metadata class.</span></span>

<span data-ttu-id="9e070-141">Na pasta **modelos** , adicione uma classe chamada *Metadata.cs*.</span><span class="sxs-lookup"><span data-stu-id="9e070-141">In the **Models** folder, add a class named *Metadata.cs*.</span></span>

<span data-ttu-id="9e070-142">Substitua o código em *Metadata.cs* pelo código a seguir.</span><span class="sxs-lookup"><span data-stu-id="9e070-142">Replace the code in *Metadata.cs* with the following code.</span></span>

[!code-csharp[Main](enhancing-data-validation/samples/sample3.cs)]

<span data-ttu-id="9e070-143">Essas classes de metadados contêm todos os atributos de validação que você aplicou anteriormente às classes de modelo.</span><span class="sxs-lookup"><span data-stu-id="9e070-143">These metadata classes contain all of the validation attributes that you had previously applied to the model classes.</span></span> <span data-ttu-id="9e070-144">O atributo de **exibição** é usado para alterar o valor usado para rótulos de texto.</span><span class="sxs-lookup"><span data-stu-id="9e070-144">The **Display** attribute is used to change the value used for text labels.</span></span>

<span data-ttu-id="9e070-145">Agora, você deve associar as classes de modelo às classes de metadados.</span><span class="sxs-lookup"><span data-stu-id="9e070-145">Now, you must associate the model classes with the metadata classes.</span></span>

<span data-ttu-id="9e070-146">Na pasta **modelos** , adicione uma classe chamada *PartialClasses.cs*.</span><span class="sxs-lookup"><span data-stu-id="9e070-146">In the **Models** folder, add a class named *PartialClasses.cs*.</span></span>

<span data-ttu-id="9e070-147">Substitua o conteúdo do arquivo pelo código a seguir.</span><span class="sxs-lookup"><span data-stu-id="9e070-147">Replace the contents of the file with the following code.</span></span>

[!code-csharp[Main](enhancing-data-validation/samples/sample4.cs)]

<span data-ttu-id="9e070-148">Observe que cada classe é marcada como uma classe de `partial`, e cada uma corresponde ao nome e ao namespace como a classe gerada automaticamente.</span><span class="sxs-lookup"><span data-stu-id="9e070-148">Notice that each class is marked as a `partial` class, and each matches the name and namespace as the class that is automatically generated.</span></span> <span data-ttu-id="9e070-149">Aplicando o atributo de metadados à classe parcial, você garante que os atributos de validação de dados serão aplicados à classe gerada automaticamente.</span><span class="sxs-lookup"><span data-stu-id="9e070-149">By applying the metadata attribute to the partial class, you ensure that the data validation attributes will be applied to the automatically-generated class.</span></span> <span data-ttu-id="9e070-150">Esses atributos não serão perdidos quando você regenerar as classes de modelo porque o atributo de metadados é aplicado em classes parciais que não são geradas novamente.</span><span class="sxs-lookup"><span data-stu-id="9e070-150">These attributes will not be lost when you regenerate the model classes because the metadata attribute is applied in partial classes that are not regenerated.</span></span>

<span data-ttu-id="9e070-151">Para regenerar as classes geradas automaticamente, abra o arquivo *ContosoModel. edmx* .</span><span class="sxs-lookup"><span data-stu-id="9e070-151">To regenerate the automatically-generated classes, open the *ContosoModel.edmx* file.</span></span> <span data-ttu-id="9e070-152">Mais uma vez, clique com o botão direito do mouse na superfície de design e selecione **atualizar modelo do banco de dados**.</span><span class="sxs-lookup"><span data-stu-id="9e070-152">Once again, right-click on the design surface and select **Update Model from Database**.</span></span> <span data-ttu-id="9e070-153">Mesmo que você não tenha alterado o banco de dados, esse processo irá regenerar as classes.</span><span class="sxs-lookup"><span data-stu-id="9e070-153">Even though you have not changed the database, this process will regenerate the classes.</span></span> <span data-ttu-id="9e070-154">Na guia **Atualizar** , selecione **tabelas** e **concluir**.</span><span class="sxs-lookup"><span data-stu-id="9e070-154">In the **Refresh** tab, select **Tables** and **Finish**.</span></span>

<span data-ttu-id="9e070-155">Salve o arquivo *ContosoModel. edmx* para aplicar as alterações.</span><span class="sxs-lookup"><span data-stu-id="9e070-155">Save the *ContosoModel.edmx* file to apply the changes.</span></span>

<span data-ttu-id="9e070-156">Abra o arquivo *Student.cs* ou o arquivo *Enrollment.cs* e observe que os atributos de validação de dados aplicados anteriormente não estão mais no arquivo.</span><span class="sxs-lookup"><span data-stu-id="9e070-156">Open the *Student.cs* file or the *Enrollment.cs* file, and notice that the data validation attributes you applied earlier are no longer in the file.</span></span> <span data-ttu-id="9e070-157">No entanto, execute o aplicativo e observe que as regras de validação ainda são aplicadas quando você insere dados.</span><span class="sxs-lookup"><span data-stu-id="9e070-157">However, run the application, and notice that the validation rules are still applied when you enter data.</span></span>

## <a name="conclusion"></a><span data-ttu-id="9e070-158">Conclusão</span><span class="sxs-lookup"><span data-stu-id="9e070-158">Conclusion</span></span>

<span data-ttu-id="9e070-159">Esta série forneceu um exemplo simples de como gerar código a partir de um banco de dados existente que permite aos usuários editar, atualizar, criar e excluí-los.</span><span class="sxs-lookup"><span data-stu-id="9e070-159">This series provided a simple example of how to generate code from an existing database that enables users to edit, update, create and delete data.</span></span> <span data-ttu-id="9e070-160">Ele usou o ASP.NET MVC 5, o Entity Framework e o ASP.NET scaffolding para criar o projeto.</span><span class="sxs-lookup"><span data-stu-id="9e070-160">It used ASP.NET MVC 5, Entity Framework and ASP.NET Scaffolding to create the project.</span></span> 

<span data-ttu-id="9e070-161">Para obter um exemplo introdutório de desenvolvimento de Code First, consulte [introdução com o ASP.NET MVC 5](../introduction/getting-started.md).</span><span class="sxs-lookup"><span data-stu-id="9e070-161">For an introductory example of Code First development, see [Getting Started with ASP.NET MVC 5](../introduction/getting-started.md).</span></span> 

<span data-ttu-id="9e070-162">Para obter um exemplo mais avançado, consulte [criando um modelo de dados de Entity Framework para um aplicativo ASP.NET MVC 4](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span><span class="sxs-lookup"><span data-stu-id="9e070-162">For a more advanced example, see [Creating an Entity Framework Data Model for an ASP.NET MVC 4 App](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span></span> <span data-ttu-id="9e070-163">Observe que a API DbContext usada para trabalhar com dados no Database First é igual à API usada para trabalhar com dados no Code First.</span><span class="sxs-lookup"><span data-stu-id="9e070-163">Note that the DbContext API that you use for working with data in Database First is the same as the API you use for working with data in Code First.</span></span> <span data-ttu-id="9e070-164">Mesmo que você pretenda usar Database First, você pode aprender a lidar com cenários mais complexos, como leitura e atualização de dados relacionados, tratamento de conflitos de simultaneidade e assim por diante de um tutorial de Code First.</span><span class="sxs-lookup"><span data-stu-id="9e070-164">Even if you intend to use Database First, you can learn how to handle more complex scenarios such as reading and updating related data, handling concurrency conflicts, and so forth from a Code First tutorial.</span></span> <span data-ttu-id="9e070-165">A única diferença está em como o banco de dados, a classe de contexto e as classes de entidade são criadas.</span><span class="sxs-lookup"><span data-stu-id="9e070-165">The only difference is in how the database, context class, and entity classes are created.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="9e070-166">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="9e070-166">Additional resources</span></span>

<span data-ttu-id="9e070-167">Para obter uma lista completa das anotações de validação de dados que você pode aplicar a propriedades e classes, consulte [System. ComponentModel. Annotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx).</span><span class="sxs-lookup"><span data-stu-id="9e070-167">For a full list of data validation annotations you can apply to properties and classes, see [System.ComponentModel.DataAnnotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx).</span></span>

## <a name="next-steps"></a><span data-ttu-id="9e070-168">Próximas etapas</span><span class="sxs-lookup"><span data-stu-id="9e070-168">Next steps</span></span>

<span data-ttu-id="9e070-169">Neste tutorial, você:</span><span class="sxs-lookup"><span data-stu-id="9e070-169">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="9e070-170">Anotações de dados adicionadas</span><span class="sxs-lookup"><span data-stu-id="9e070-170">Added data annotations</span></span>
> * <span data-ttu-id="9e070-171">Classes de metadados adicionadas</span><span class="sxs-lookup"><span data-stu-id="9e070-171">Added metadata classes</span></span>

<span data-ttu-id="9e070-172">Para saber como implantar um aplicativo Web e um banco de dados SQL no serviço Azure App, consulte este tutorial:</span><span class="sxs-lookup"><span data-stu-id="9e070-172">To learn how to deploy a web app and SQL database to Azure App Service, see this tutorial:</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="9e070-173">Implantar um aplicativo .NET no serviço Azure App</span><span class="sxs-lookup"><span data-stu-id="9e070-173">Deploy a .NET app to Azure App Service</span></span>](/azure/app-service/app-service-web-tutorial-dotnet-sqldatabase/)
