---
uid: web-forms/overview/presenting-and-managing-data/model-binding/adding-business-logic-layer
title: Adicionando a camada de lógica de negócios a um projeto que usa associação de modelo e formulários da Web | Microsoft Docs
author: Rick-Anderson
description: Esta série de tutoriais demonstra os aspectos básicos do uso de associação de modelo com um projeto ASP.NET Web Forms. A associação de modelo torna a interação de dados mais direta-...
ms.author: riande
ms.date: 02/27/2014
ms.assetid: 7ef664b3-1cc8-4cbf-bb18-9f0f3a3ada2b
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/adding-business-logic-layer
msc.type: authoredcontent
ms.openlocfilehash: a824d06d3781e11706f2a48d44ea3ad89bdb7c8b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78634830"
---
# <a name="adding-business-logic-layer-to-a-project-that-uses-model-binding-and-web-forms"></a><span data-ttu-id="3216c-104">Adicionando a camada de lógica de negócios a um projeto que usa associação de modelo e formulários da Web</span><span class="sxs-lookup"><span data-stu-id="3216c-104">Adding business logic layer to a project that uses model binding and web forms</span></span>

<span data-ttu-id="3216c-105">por [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="3216c-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="3216c-106">Esta série de tutoriais demonstra os aspectos básicos do uso de associação de modelo com um projeto ASP.NET Web Forms.</span><span class="sxs-lookup"><span data-stu-id="3216c-106">This tutorial series demonstrates basic aspects of using model binding with an ASP.NET Web Forms project.</span></span> <span data-ttu-id="3216c-107">A associação de modelo torna a interação de dados mais direta do que lidar com objetos de fonte de dados (como ObjectDataSource ou SqlDataSource).</span><span class="sxs-lookup"><span data-stu-id="3216c-107">Model binding makes data interaction more straight-forward than dealing with data source objects (such as ObjectDataSource or SqlDataSource).</span></span> <span data-ttu-id="3216c-108">Esta série começa com material introdutório e passa para conceitos mais avançados em Tutoriais posteriores.</span><span class="sxs-lookup"><span data-stu-id="3216c-108">This series starts with introductory material and moves to more advanced concepts in later tutorials.</span></span>
> 
> <span data-ttu-id="3216c-109">Este tutorial mostra como usar a associação de modelo com uma camada de lógica de negócios.</span><span class="sxs-lookup"><span data-stu-id="3216c-109">This tutorial shows how to use model binding with a business logic layer.</span></span> <span data-ttu-id="3216c-110">Você definirá o membro OnCallingDataMethods para especificar que um objeto diferente da página atual é usado para chamar os métodos de dados.</span><span class="sxs-lookup"><span data-stu-id="3216c-110">You will set the OnCallingDataMethods member to specify that an object other than the current page is used to call the data methods.</span></span>
> 
> <span data-ttu-id="3216c-111">Este tutorial se baseia no projeto criado nas partes [anteriores](retrieving-data.md) da série.</span><span class="sxs-lookup"><span data-stu-id="3216c-111">This tutorial builds on the project created in the [earlier](retrieving-data.md) parts of the series.</span></span>
> 
> <span data-ttu-id="3216c-112">Você pode [baixar](https://go.microsoft.com/fwlink/?LinkId=286116) o projeto completo no C# ou no VB.</span><span class="sxs-lookup"><span data-stu-id="3216c-112">You can [download](https://go.microsoft.com/fwlink/?LinkId=286116) the complete project in C# or VB.</span></span> <span data-ttu-id="3216c-113">O código para download funciona com o Visual Studio 2012 ou Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="3216c-113">The downloadable code works with either Visual Studio 2012 or Visual Studio 2013.</span></span> <span data-ttu-id="3216c-114">Ele usa o modelo do Visual Studio 2012, que é um pouco diferente do modelo de Visual Studio 2013 mostrado neste tutorial.</span><span class="sxs-lookup"><span data-stu-id="3216c-114">It uses the Visual Studio 2012 template, which is slightly different than the Visual Studio 2013 template shown in this tutorial.</span></span>

## <a name="what-youll-build"></a><span data-ttu-id="3216c-115">O que você criará</span><span class="sxs-lookup"><span data-stu-id="3216c-115">What you'll build</span></span>

<span data-ttu-id="3216c-116">A associação de modelo permite colocar o código de interação de dados no arquivo code-behind para uma página da Web ou em uma classe lógica de negócios separada.</span><span class="sxs-lookup"><span data-stu-id="3216c-116">Model binding enables you to put your data interaction code in either the code-behind file for a web page or in a separate business logic class.</span></span> <span data-ttu-id="3216c-117">Os tutoriais anteriores mostraram como usar os arquivos code-behind para o código de interação de dados.</span><span class="sxs-lookup"><span data-stu-id="3216c-117">The previous tutorials have shown how to use the code-behind files for data interaction code.</span></span> <span data-ttu-id="3216c-118">Essa abordagem funciona para sites pequenos, mas pode levar à repetição de código e maior dificuldade ao manter um site grande.</span><span class="sxs-lookup"><span data-stu-id="3216c-118">This approach works for small sites but it can lead to code repetition and greater difficulty when maintaining a large site.</span></span> <span data-ttu-id="3216c-119">Também pode ser muito difícil testar programaticamente o código que reside em arquivos code-behind porque não há nenhuma camada de abstração.</span><span class="sxs-lookup"><span data-stu-id="3216c-119">It can also be very difficult to programmatically test code that resides in code behind files because there is no abstraction layer.</span></span>

<span data-ttu-id="3216c-120">Para centralizar o código de interação de dados, você pode criar uma camada de lógica de negócios que contenha toda a lógica para interagir com os dados.</span><span class="sxs-lookup"><span data-stu-id="3216c-120">To centralize the data interaction code, you can create a business logic layer that contains all of the logic for interacting with data.</span></span> <span data-ttu-id="3216c-121">Em seguida, você chama a camada de lógica de negócios em suas páginas da Web.</span><span class="sxs-lookup"><span data-stu-id="3216c-121">You then call the business logic layer from your web pages.</span></span> <span data-ttu-id="3216c-122">Este tutorial mostra como mover todo o código que você escreveu nos tutoriais anteriores em uma camada de lógica de negócios e, em seguida, usar esse código nas páginas.</span><span class="sxs-lookup"><span data-stu-id="3216c-122">This tutorial shows how to move all of the code you have written in the previous tutorials into a business logic layer, and then use that code from the pages.</span></span>

<span data-ttu-id="3216c-123">Neste tutorial, você aprenderá a:</span><span class="sxs-lookup"><span data-stu-id="3216c-123">In this tutorial, you'll:</span></span>

1. <span data-ttu-id="3216c-124">Mover o código dos arquivos code-behind para uma camada de lógica de negócios</span><span class="sxs-lookup"><span data-stu-id="3216c-124">Move the code from code-behind files to a business logic layer</span></span>
2. <span data-ttu-id="3216c-125">Alterar os controles associados a dados para chamar os métodos na camada de lógica de negócios</span><span class="sxs-lookup"><span data-stu-id="3216c-125">Change your data bound controls to call the methods in the business logic layer</span></span>

## <a name="create-business-logic-layer"></a><span data-ttu-id="3216c-126">Criar camada de lógica de negócios</span><span class="sxs-lookup"><span data-stu-id="3216c-126">Create business logic layer</span></span>

<span data-ttu-id="3216c-127">Agora, você criará a classe que é chamada a partir das páginas da Web.</span><span class="sxs-lookup"><span data-stu-id="3216c-127">Now, you will create the class that is called from the web pages.</span></span> <span data-ttu-id="3216c-128">Os métodos nessa classe são semelhantes aos métodos usados nos tutoriais anteriores e incluem os atributos do provedor de valor.</span><span class="sxs-lookup"><span data-stu-id="3216c-128">The methods in this class look similar to the methods you used in the previous tutorials and include the value provider attributes.</span></span>

<span data-ttu-id="3216c-129">Primeiro, adicione uma nova pasta chamada **BLL**.</span><span class="sxs-lookup"><span data-stu-id="3216c-129">First, add a new folder called **BLL**.</span></span>

![Adicionar pasta](adding-business-logic-layer/_static/image1.png)

<span data-ttu-id="3216c-131">Na pasta BLL, crie uma nova classe chamada **SchoolBL.cs**.</span><span class="sxs-lookup"><span data-stu-id="3216c-131">In the BLL folder, create a new class named **SchoolBL.cs**.</span></span> <span data-ttu-id="3216c-132">Ele conterá todas as operações de dados que residiram originalmente nos arquivos code-behind.</span><span class="sxs-lookup"><span data-stu-id="3216c-132">It will contain all of the data operations that originally resided in code-behind files.</span></span> <span data-ttu-id="3216c-133">Os métodos são quase iguais aos métodos no arquivo code-behind, mas incluirão algumas alterações.</span><span class="sxs-lookup"><span data-stu-id="3216c-133">The methods are almost the same as the methods in the code-behind file, but will include some changes.</span></span>

<span data-ttu-id="3216c-134">A alteração mais importante a ser observada é que você não está mais executando o código de dentro de uma instância da classe de **página** .</span><span class="sxs-lookup"><span data-stu-id="3216c-134">The most important change to note is that you are no longer executing the code from within an instance of **Page** class.</span></span> <span data-ttu-id="3216c-135">A classe Page contém o método **TryUpdateModel** e a propriedade **ModelState** .</span><span class="sxs-lookup"><span data-stu-id="3216c-135">The Page class contains the **TryUpdateModel** method and the **ModelState** property.</span></span> <span data-ttu-id="3216c-136">Quando esse código é movido para uma camada de lógica de negócios, você não tem mais uma instância da classe de página para chamar esses membros.</span><span class="sxs-lookup"><span data-stu-id="3216c-136">When this code is moved to a business logic layer, you no longer have an instance of the Page class to call these members.</span></span> <span data-ttu-id="3216c-137">Para contornar esse problema, você deve adicionar um parâmetro **ModelMethodContext** a qualquer método que acesse TryUpdateModel ou ModelState.</span><span class="sxs-lookup"><span data-stu-id="3216c-137">To get around this issue, you must add a **ModelMethodContext** parameter to any method that accesses TryUpdateModel or ModelState.</span></span> <span data-ttu-id="3216c-138">Use esse parâmetro ModelMethodContext para chamar TryUpdateModel ou recuperar ModelState.</span><span class="sxs-lookup"><span data-stu-id="3216c-138">You use this ModelMethodContext parameter to call TryUpdateModel or retrieve ModelState.</span></span> <span data-ttu-id="3216c-139">Você não precisa alterar nada na página da Web para considerar esse novo parâmetro.</span><span class="sxs-lookup"><span data-stu-id="3216c-139">You do not need to change anything in the web page to account for this new parameter.</span></span>

<span data-ttu-id="3216c-140">Substitua o código em SchoolBL.cs pelo código a seguir.</span><span class="sxs-lookup"><span data-stu-id="3216c-140">Replace the code in SchoolBL.cs with the following code.</span></span>

[!code-csharp[Main](adding-business-logic-layer/samples/sample1.cs)]

## <a name="revise-existing-pages-to-retrieve-data-from-business-logic-layer"></a><span data-ttu-id="3216c-141">Revisar páginas existentes para recuperar dados da camada de lógica de negócios</span><span class="sxs-lookup"><span data-stu-id="3216c-141">Revise existing pages to retrieve data from business logic layer</span></span>

<span data-ttu-id="3216c-142">Por fim, você converterá as páginas students. aspx, addstudent. aspx e courses. aspx de usar consultas no arquivo code-behind para usar a camada de lógica de negócios.</span><span class="sxs-lookup"><span data-stu-id="3216c-142">Finally, you will convert the pages Students.aspx, AddStudent.aspx, and Courses.aspx from using queries in the code-behind file to using the business logic layer.</span></span>

<span data-ttu-id="3216c-143">Nos arquivos code-behind para estudantes, mystudent e cursos, exclua ou comente os seguintes métodos de consulta:</span><span class="sxs-lookup"><span data-stu-id="3216c-143">In the code-behind files for Students, AddStudent, and Courses, delete or comment out the following query methods:</span></span>

- <span data-ttu-id="3216c-144">studentsGrid\_GetData</span><span class="sxs-lookup"><span data-stu-id="3216c-144">studentsGrid\_GetData</span></span>
- <span data-ttu-id="3216c-145">studentsGrid\_UpdateItem</span><span class="sxs-lookup"><span data-stu-id="3216c-145">studentsGrid\_UpdateItem</span></span>
- <span data-ttu-id="3216c-146">studentsGrid\_DeleteItem</span><span class="sxs-lookup"><span data-stu-id="3216c-146">studentsGrid\_DeleteItem</span></span>
- <span data-ttu-id="3216c-147">addStudentForm\_InsertItem</span><span class="sxs-lookup"><span data-stu-id="3216c-147">addStudentForm\_InsertItem</span></span>
- <span data-ttu-id="3216c-148">coursesGrid\_GetData</span><span class="sxs-lookup"><span data-stu-id="3216c-148">coursesGrid\_GetData</span></span>

<span data-ttu-id="3216c-149">Agora você não deve ter nenhum código no arquivo code-behind que pertença a operações de dados.</span><span class="sxs-lookup"><span data-stu-id="3216c-149">You now should have no code in the code-behind file that pertains to data operations.</span></span>

<span data-ttu-id="3216c-150">O manipulador de eventos **OnCallingDataMethods** permite que você especifique um objeto a ser usado para os métodos de dados.</span><span class="sxs-lookup"><span data-stu-id="3216c-150">The **OnCallingDataMethods** event handler enables you to specify an object to use for the data methods.</span></span> <span data-ttu-id="3216c-151">Em students. aspx, adicione um valor para esse manipulador de eventos e altere os nomes dos métodos de dados para os nomes dos métodos na classe lógica de negócios.</span><span class="sxs-lookup"><span data-stu-id="3216c-151">In Students.aspx, add a value for that event handler and change the names of the data methods to the names of the methods in the business logic class.</span></span>

[!code-aspx[Main](adding-business-logic-layer/samples/sample2.aspx?highlight=3-4,8)]

<span data-ttu-id="3216c-152">No arquivo code-behind para students. aspx, defina o manipulador de eventos para o evento CallingDataMethods.</span><span class="sxs-lookup"><span data-stu-id="3216c-152">In the code-behind file for Students.aspx, define the event handler for the CallingDataMethods event.</span></span> <span data-ttu-id="3216c-153">Nesse manipulador de eventos, você especifica a classe de lógica de negócios para operações de dados.</span><span class="sxs-lookup"><span data-stu-id="3216c-153">In this event handler, you specify the business logic class for data operations.</span></span>

[!code-csharp[Main](adding-business-logic-layer/samples/sample3.cs)]

<span data-ttu-id="3216c-154">Em addstudent. aspx, faça alterações semelhantes.</span><span class="sxs-lookup"><span data-stu-id="3216c-154">In AddStudent.aspx, make similar changes.</span></span>

[!code-aspx[Main](adding-business-logic-layer/samples/sample4.aspx?highlight=3-4)]

[!code-csharp[Main](adding-business-logic-layer/samples/sample5.cs)]

<span data-ttu-id="3216c-155">Em cursos. aspx, faça alterações semelhantes.</span><span class="sxs-lookup"><span data-stu-id="3216c-155">In Courses.aspx, make similar changes.</span></span>

[!code-aspx[Main](adding-business-logic-layer/samples/sample6.aspx?highlight=3-4)]

[!code-csharp[Main](adding-business-logic-layer/samples/sample7.cs)]

<span data-ttu-id="3216c-156">Execute o aplicativo e observe que todas as páginas funcionam como estavam anteriormente.</span><span class="sxs-lookup"><span data-stu-id="3216c-156">Run the application and notice that all of the pages function as they had previously.</span></span> <span data-ttu-id="3216c-157">A lógica de validação também funciona corretamente.</span><span class="sxs-lookup"><span data-stu-id="3216c-157">The validation logic also works correctly.</span></span>

## <a name="conclusion"></a><span data-ttu-id="3216c-158">Conclusão</span><span class="sxs-lookup"><span data-stu-id="3216c-158">Conclusion</span></span>

<span data-ttu-id="3216c-159">Neste tutorial, você reestruturará seu aplicativo para usar uma camada de acesso a dados e a lógica de negócios.</span><span class="sxs-lookup"><span data-stu-id="3216c-159">In this tutorial, you re-structured your application to use a data access layer and business logic layer.</span></span> <span data-ttu-id="3216c-160">Você especificou que os controles de dados usam um objeto que não é a página atual para operações de dados.</span><span class="sxs-lookup"><span data-stu-id="3216c-160">You specified that the data controls use an object that is not the current page for data operations.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="3216c-161">Anterior</span><span class="sxs-lookup"><span data-stu-id="3216c-161">Previous</span></span>](using-query-string-values-to-retrieve-data.md)
