---
uid: mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3
title: Usando o calendário de pop-up DatePicker do HTML5 e jQuery UI com o ASP.NET MVC-parte 3 | Microsoft Docs
author: Rick-Anderson
description: Este tutorial ensinará a você noções básicas de como trabalhar com modelos do editor, modelos de exibição e o calendário de pop-up do jQuery UI DatePicker em um ASP.NET MV...
ms.author: riande
ms.date: 08/29/2011
ms.assetid: 8f5f91ae-12d7-4cf3-ac09-4bb53d07ee60
msc.legacyurl: /mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3
msc.type: authoredcontent
ms.openlocfilehash: b3249397e54e64538c4dc78e5fe8b94656e8962b
ms.sourcegitcommit: 7709c0a091b8d55b7b33bad8849f7b66b23c3d72
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/19/2020
ms.locfileid: "77457889"
---
# <a name="using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc---part-3"></a><span data-ttu-id="9226c-103">Usando o calendário de pop-up DatePicker do HTML5 e jQuery UI com o ASP.NET MVC-parte 3</span><span class="sxs-lookup"><span data-stu-id="9226c-103">Using the HTML5 and jQuery UI Datepicker Popup Calendar with ASP.NET MVC - Part 3</span></span>

<span data-ttu-id="9226c-104">por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="9226c-104">by [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

> <span data-ttu-id="9226c-105">Este tutorial ensinará a você noções básicas de como trabalhar com modelos do editor, modelos de exibição e o calendário de pop-up do jQuery UI DatePicker em um aplicativo Web ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="9226c-105">This tutorial will teach you the basics of how to work with editor templates, display templates, and the jQuery UI datepicker popup calendar in an ASP.NET MVC Web application.</span></span>

## <a name="working-with-complex-types"></a><span data-ttu-id="9226c-106">Trabalhando com tipos complexos</span><span class="sxs-lookup"><span data-stu-id="9226c-106">Working with Complex Types</span></span>

<span data-ttu-id="9226c-107">Nesta seção, você criará uma classe de endereço e aprenderá a criar um modelo para exibi-lo.</span><span class="sxs-lookup"><span data-stu-id="9226c-107">In this section you'll create an address class and learn how to create a template to display it.</span></span>

<span data-ttu-id="9226c-108">Na pasta *modelos* , crie um novo arquivo de classe chamado *Person.cs* , onde você colocará dois tipos: uma classe `Person` e uma classe `Address`.</span><span class="sxs-lookup"><span data-stu-id="9226c-108">In the *Models* folder, create a new class file named *Person.cs* where you'll put two types: a `Person` class and an `Address` class.</span></span> <span data-ttu-id="9226c-109">A classe `Person` conterá uma propriedade que é digitada como `Address`.</span><span class="sxs-lookup"><span data-stu-id="9226c-109">The `Person` class will contain a property that's typed as `Address`.</span></span> <span data-ttu-id="9226c-110">O tipo de `Address` é um tipo complexo, o que significa que não é um dos tipos internos, como `int`, `string`ou `double`.</span><span class="sxs-lookup"><span data-stu-id="9226c-110">The `Address` type is a complex type, meaning it's not one of the built-in types like `int`, `string`, or `double`.</span></span> <span data-ttu-id="9226c-111">Em vez disso, ele tem várias propriedades.</span><span class="sxs-lookup"><span data-stu-id="9226c-111">Instead, it has several properties.</span></span> <span data-ttu-id="9226c-112">O código para as novas classes tem a seguinte aparência:</span><span class="sxs-lookup"><span data-stu-id="9226c-112">The code for the new classes looks like this:</span></span>

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample1.cs)]

<span data-ttu-id="9226c-113">No controlador de `Movie`, adicione a seguinte ação de `PersonDetail` para exibir uma instância Person:</span><span class="sxs-lookup"><span data-stu-id="9226c-113">In the `Movie` controller, add the following `PersonDetail` action to display a person instance:</span></span>

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample2.cs)]

<span data-ttu-id="9226c-114">Em seguida, adicione o seguinte código ao controlador de `Movie` para preencher o modelo de `Person` com alguns dados de exemplo:</span><span class="sxs-lookup"><span data-stu-id="9226c-114">Then add the following code to the `Movie` controller to populate the `Person` model with some sample data:</span></span>

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample3.cs)]

<span data-ttu-id="9226c-115">Abra o arquivo *Views\Movies\PersonDetail.cshtml* e adicione a marcação a seguir para a exibição `PersonDetail`.</span><span class="sxs-lookup"><span data-stu-id="9226c-115">Open the *Views\Movies\PersonDetail.cshtml* file and add the following markup for the `PersonDetail` view.</span></span>

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample4.cshtml)]

<span data-ttu-id="9226c-116">Pressione CTRL + F5 para executar o aplicativo e navegue até *filmes/PersonDetail*.</span><span class="sxs-lookup"><span data-stu-id="9226c-116">Press Ctrl+F5 to run the application and navigate to *Movies/PersonDetail*.</span></span>

<span data-ttu-id="9226c-117">A exibição `PersonDetail` não contém o `Address` tipo complexo, como você pode ver nesta captura de tela.</span><span class="sxs-lookup"><span data-stu-id="9226c-117">The `PersonDetail` view doesn't contain the `Address` complex type, as you can see in this screenshot.</span></span> <span data-ttu-id="9226c-118">(Nenhum endereço é mostrado.)</span><span class="sxs-lookup"><span data-stu-id="9226c-118">(No address is shown.)</span></span>

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/_static/image1.png)

<span data-ttu-id="9226c-119">Os dados do modelo de `Address` não são exibidos porque ele é um tipo complexo.</span><span class="sxs-lookup"><span data-stu-id="9226c-119">The `Address` model data is not displayed because it's a complex type.</span></span> <span data-ttu-id="9226c-120">Para exibir as informações de endereço, abra o arquivo *Views\Movies\PersonDetail.cshtml* novamente e adicione a marcação a seguir.</span><span class="sxs-lookup"><span data-stu-id="9226c-120">To display the address information, open the *Views\Movies\PersonDetail.cshtml* file again and add the following markup.</span></span>

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample5.cshtml)]

<span data-ttu-id="9226c-121">A marcação completa para o modo de exibição de `PersonDetail` agora tem a seguinte aparência:</span><span class="sxs-lookup"><span data-stu-id="9226c-121">The complete markup for the `PersonDetail` now view looks like this:</span></span>

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample6.cshtml)]

<span data-ttu-id="9226c-122">Execute o aplicativo novamente e exiba a exibição `PersonDetail`.</span><span class="sxs-lookup"><span data-stu-id="9226c-122">Run the application again and display the `PersonDetail` view.</span></span> <span data-ttu-id="9226c-123">As informações de endereço agora são exibidas:</span><span class="sxs-lookup"><span data-stu-id="9226c-123">The address information is now displayed:</span></span>

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/_static/image2.png)

### <a name="creating-a-template-for-a-complex-type"></a><span data-ttu-id="9226c-124">Criando um modelo para um tipo complexo</span><span class="sxs-lookup"><span data-stu-id="9226c-124">Creating a Template for a Complex Type</span></span>

<span data-ttu-id="9226c-125">Nesta seção, você criará um modelo que será usado para renderizar o `Address` tipo complexo.</span><span class="sxs-lookup"><span data-stu-id="9226c-125">In this section you'll create a template that will be used to render the `Address` complex type.</span></span> <span data-ttu-id="9226c-126">Quando você cria um modelo para o tipo de `Address`, o ASP.NET MVC pode usá-lo automaticamente para formatar um modelo de endereço em qualquer lugar no aplicativo.</span><span class="sxs-lookup"><span data-stu-id="9226c-126">When you create a template for the `Address` type, ASP.NET MVC can automatically use it to format an address model anywhere in the application.</span></span> <span data-ttu-id="9226c-127">Isso lhe dá uma maneira de controlar a renderização do tipo de `Address` de apenas um lugar no aplicativo.</span><span class="sxs-lookup"><span data-stu-id="9226c-127">This gives you a way to control the rendering of the `Address` type from just one place in the application.</span></span>

<span data-ttu-id="9226c-128">Na pasta *Views\Shared\DisplayTemplates* , crie um modo de exibição parcial com rigidez de tipos chamado **endereço**:</span><span class="sxs-lookup"><span data-stu-id="9226c-128">In the *Views\Shared\DisplayTemplates* folder, create a strongly typed partial view named **Address**:</span></span>

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/_static/image3.png)

<span data-ttu-id="9226c-129">Clique em **Adicionar**e, em seguida, abra o novo arquivo *Views\Shared\DisplayTemplates\Address.cshtml* .</span><span class="sxs-lookup"><span data-stu-id="9226c-129">Click **Add**, and then open the new *Views\Shared\DisplayTemplates\Address.cshtml* file.</span></span> <span data-ttu-id="9226c-130">A nova exibição contém a seguinte marcação gerada:</span><span class="sxs-lookup"><span data-stu-id="9226c-130">The new view contains the following generated markup:</span></span>

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample7.cshtml)]

<span data-ttu-id="9226c-131">Execute o aplicativo e exiba a exibição `PersonDetail`.</span><span class="sxs-lookup"><span data-stu-id="9226c-131">Run the application and display the `PersonDetail` view.</span></span> <span data-ttu-id="9226c-132">Desta vez, o modelo de `Address` que você acabou de criar é usado para exibir o `Address` tipo complexo, portanto, a exibição é semelhante ao seguinte:</span><span class="sxs-lookup"><span data-stu-id="9226c-132">This time, the `Address` template that you just created is used to display the `Address` complex type, so the display looks like the following:</span></span>

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/_static/image4.png)

### <a name="summary-ways-to-specify-the-model-display-format-and-template"></a><span data-ttu-id="9226c-133">Resumo: maneiras de especificar o modelo e o formato de exibição do modelo</span><span class="sxs-lookup"><span data-stu-id="9226c-133">Summary: Ways to Specify the Model Display Format and Template</span></span>

<span data-ttu-id="9226c-134">Você viu que pode especificar o formato ou o modelo para uma propriedade de modelo usando as seguintes abordagens:</span><span class="sxs-lookup"><span data-stu-id="9226c-134">You've seen that you can specify the format or template for a model property by using the following approaches:</span></span>

- <span data-ttu-id="9226c-135">Aplicando o atributo `DisplayFormat` a uma propriedade no modelo.</span><span class="sxs-lookup"><span data-stu-id="9226c-135">Applying the `DisplayFormat` attribute to a property in the model.</span></span> <span data-ttu-id="9226c-136">Por exemplo, o código a seguir faz com que a data seja exibida sem a hora:</span><span class="sxs-lookup"><span data-stu-id="9226c-136">For example, the following code causes the date to be displayed without the time:</span></span>

    [!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample8.cs)]
- <span data-ttu-id="9226c-137">Aplicar um atributo [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) a uma propriedade no modelo e especificar o tipo de dados.</span><span class="sxs-lookup"><span data-stu-id="9226c-137">Applying a [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) attribute to a property in the model and specifying the data type.</span></span> <span data-ttu-id="9226c-138">Por exemplo, o código a seguir faz com que a data seja exibida sem a hora.</span><span class="sxs-lookup"><span data-stu-id="9226c-138">For example, the following code causes the date to be displayed without the time.</span></span>

    [!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample9.cs)]

    <span data-ttu-id="9226c-139">Se o aplicativo contiver um modelo *Date. cshtml* na pasta *Views\Shared\DisplayTemplates* ou na pasta *Views\Movies\DisplayTemplates* , esse modelo será usado para renderizar a propriedade `DateTime`.</span><span class="sxs-lookup"><span data-stu-id="9226c-139">If the application contains a *date.cshtml* template in the *Views\Shared\DisplayTemplates* folder or the *Views\Movies\DisplayTemplates* folder, that template will be used to render the `DateTime` property.</span></span> <span data-ttu-id="9226c-140">Caso contrário, o sistema interno de modelagem de ASP.NET exibe a propriedade como uma data.</span><span class="sxs-lookup"><span data-stu-id="9226c-140">Otherwise the built-in ASP.NET templating system displays the property as a date.</span></span>
- <span data-ttu-id="9226c-141">Criar um modelo de exibição na pasta *Views\Shared\DisplayTemplates* ou na pasta *Views\Movies\DisplayTemplates* cujo nome corresponde ao tipo de dados que você deseja formatar.</span><span class="sxs-lookup"><span data-stu-id="9226c-141">Creating a display template in the *Views\Shared\DisplayTemplates* folder or the *Views\Movies\DisplayTemplates* folder whose name matches the data type that you want to format.</span></span> <span data-ttu-id="9226c-142">Por exemplo, você viu que o *Views\Shared\DisplayTemplates\DateTime.cshtml* foi usado para processar propriedades de `DateTime` em um modelo, sem adicionar um atributo ao modelo e sem adicionar nenhuma marcação a exibições.</span><span class="sxs-lookup"><span data-stu-id="9226c-142">For example, you saw that the *Views\Shared\DisplayTemplates\DateTime.cshtml* was used to render `DateTime` properties in a model, without adding an attribute to the model and without adding any markup to views.</span></span>
- <span data-ttu-id="9226c-143">Usando o atributo [UIHint](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.uihintattribute.uihint.aspx) no modelo para especificar o modelo para exibir a propriedade do modelo.</span><span class="sxs-lookup"><span data-stu-id="9226c-143">Using the [UIHint](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.uihintattribute.uihint.aspx) attribute on the model to specify the template to display the model property.</span></span>
- <span data-ttu-id="9226c-144">Adicionar explicitamente o nome do modelo de exibição à chamada [HTML. DisplayFor](https://msdn.microsoft.com/library/ee407420.aspx) em uma exibição.</span><span class="sxs-lookup"><span data-stu-id="9226c-144">Explicitly adding the display template name to the [Html.DisplayFor](https://msdn.microsoft.com/library/ee407420.aspx) call in a view.</span></span>

<span data-ttu-id="9226c-145">A abordagem usada depende do que você precisa fazer em seu aplicativo.</span><span class="sxs-lookup"><span data-stu-id="9226c-145">The approach you use depends on what you need to do in your application.</span></span> <span data-ttu-id="9226c-146">Não é incomum misturar essas abordagens para obter exatamente o tipo de formatação de que você precisa.</span><span class="sxs-lookup"><span data-stu-id="9226c-146">It's not uncommon to mix these approaches to get exactly the kind of formatting that you need.</span></span>

<span data-ttu-id="9226c-147">Na próxima seção, você alternará um pouco e passará da personalização de como os dados são exibidos para personalizar como eles são inseridos.</span><span class="sxs-lookup"><span data-stu-id="9226c-147">In the next section, you'll switch gears a bit and move from customizing how data is displayed to customizing how it's entered.</span></span> <span data-ttu-id="9226c-148">Você conectará o jQuery DatePicker às exibições de edição no aplicativo para fornecer uma maneira ágil de especificar datas.</span><span class="sxs-lookup"><span data-stu-id="9226c-148">You'll hook up the jQuery datepicker to the edit views in the application in order to provide a slick way to specify dates.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="9226c-149">[Anterior](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2.md)
> [Próximo](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4.md)</span><span class="sxs-lookup"><span data-stu-id="9226c-149">[Previous](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2.md)
[Next](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4.md)</span></span>
