---
uid: web-pages/overview/ui-layouts-and-themes/validating-user-input-in-aspnet-web-pages-sites
title: Validando a entrada do usuário em sites Páginas da Web do ASP.NET (Razor) | Microsoft Docs
author: Rick-Anderson
description: Este artigo discute como validar as informações que você obtém dos usuários &mdash; ou seja, para garantir que os usuários insiram informações válidas em formulários HTML em um como...
ms.author: riande
ms.date: 02/20/2014
ms.assetid: 4eb060cc-cf14-41ae-bab1-14a2c15332d0
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/validating-user-input-in-aspnet-web-pages-sites
msc.type: authoredcontent
ms.openlocfilehash: e6f8e1051d09d11f1756bfada44a73ba7c2a1db2
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78563500"
---
# <a name="validating-user-input-in-aspnet-web-pages-razor-sites"></a><span data-ttu-id="5c614-103">Validando a entrada do usuário em sites Páginas da Web do ASP.NET (Razor)</span><span class="sxs-lookup"><span data-stu-id="5c614-103">Validating User Input in ASP.NET Web Pages (Razor) Sites</span></span>

<span data-ttu-id="5c614-104">por [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="5c614-104">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="5c614-105">Este artigo discute como validar as informações que você obtém dos usuários &mdash; ou seja, para garantir que os usuários insiram informações válidas em formulários HTML em um site do Páginas da Web do ASP.NET (Razor).</span><span class="sxs-lookup"><span data-stu-id="5c614-105">This article discusses how to validate information you get from users &mdash; that is, to make sure that users enter valid information in HTML forms in an ASP.NET Web Pages (Razor) site.</span></span>
> 
> <span data-ttu-id="5c614-106">O que você aprenderá:</span><span class="sxs-lookup"><span data-stu-id="5c614-106">What you'll learn:</span></span>
> 
> - <span data-ttu-id="5c614-107">Como verificar se a entrada de um usuário corresponde aos critérios de validação definidos por você.</span><span class="sxs-lookup"><span data-stu-id="5c614-107">How to check that a user's input matches validation criteria that you define.</span></span>
> - <span data-ttu-id="5c614-108">Como determinar se todos os testes de validação passaram.</span><span class="sxs-lookup"><span data-stu-id="5c614-108">How to determine whether all validation tests have passed.</span></span>
> - <span data-ttu-id="5c614-109">Como exibir erros de validação (e como formatá-los).</span><span class="sxs-lookup"><span data-stu-id="5c614-109">How to display validation errors (and how to format them).</span></span>
> - <span data-ttu-id="5c614-110">Como validar dados que não são fornecidos diretamente dos usuários.</span><span class="sxs-lookup"><span data-stu-id="5c614-110">How to validate data that doesn't come directly from users.</span></span>
> 
> <span data-ttu-id="5c614-111">Estes são os conceitos de programação do ASP.NET introduzidos no artigo:</span><span class="sxs-lookup"><span data-stu-id="5c614-111">These are the ASP.NET programming concepts introduced in the article:</span></span>
> 
> - <span data-ttu-id="5c614-112">O auxiliar de `Validation`.</span><span class="sxs-lookup"><span data-stu-id="5c614-112">The `Validation` helper.</span></span>
> - <span data-ttu-id="5c614-113">Os métodos `Html.ValidationSummary` e `Html.ValidationMessage`.</span><span class="sxs-lookup"><span data-stu-id="5c614-113">The `Html.ValidationSummary` and `Html.ValidationMessage` methods.</span></span>
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="5c614-114">Versões de software usadas no tutorial</span><span class="sxs-lookup"><span data-stu-id="5c614-114">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="5c614-115">Páginas da Web do ASP.NET (Razor) 3</span><span class="sxs-lookup"><span data-stu-id="5c614-115">ASP.NET Web Pages (Razor) 3</span></span>
>   
> 
> <span data-ttu-id="5c614-116">Este tutorial também funciona com o Páginas da Web do ASP.NET 2.</span><span class="sxs-lookup"><span data-stu-id="5c614-116">This tutorial also works with ASP.NET Web Pages 2.</span></span>

<span data-ttu-id="5c614-117">Este artigo inclui as seções a seguir:</span><span class="sxs-lookup"><span data-stu-id="5c614-117">This article contains the following sections:</span></span>

- [<span data-ttu-id="5c614-118">Visão geral da validação de entrada do usuário</span><span class="sxs-lookup"><span data-stu-id="5c614-118">Overview of User Input Validation</span></span>](#Overview_of_User_Input_Validation)
- [<span data-ttu-id="5c614-119">Validando entradas de usuário</span><span class="sxs-lookup"><span data-stu-id="5c614-119">Validating User Input</span></span>](#Validating_User_Input)
- [<span data-ttu-id="5c614-120">Adicionando validação do lado do cliente</span><span class="sxs-lookup"><span data-stu-id="5c614-120">Adding Client-Side Validation</span></span>](#Adding_Client-Side_Validation)
- [<span data-ttu-id="5c614-121">Erros de validação de formatação</span><span class="sxs-lookup"><span data-stu-id="5c614-121">Formatting Validation Errors</span></span>](#Formatting_Validation_Errors)
- [<span data-ttu-id="5c614-122">Validando dados que não são fornecidos diretamente dos usuários</span><span class="sxs-lookup"><span data-stu-id="5c614-122">Validating Data That Doesn't Come Directly from Users</span></span>](#Validating_Data_That_Doesnt_Come_Directly_from_Users)

<a id="Overview_of_User_Input_Validation"></a>
## <a name="overview-of-user-input-validation"></a><span data-ttu-id="5c614-123">Visão geral da validação de entrada do usuário</span><span class="sxs-lookup"><span data-stu-id="5c614-123">Overview of User Input Validation</span></span>

<span data-ttu-id="5c614-124">Se você solicitar que os usuários insiram informações em uma página — por exemplo, em um formulário — é importante certificar-se de que os valores que eles inserem são válidos.</span><span class="sxs-lookup"><span data-stu-id="5c614-124">If you ask users to enter information in a page — for example, into a form — it's important to make sure that the values that they enter are valid.</span></span> <span data-ttu-id="5c614-125">Por exemplo, você não deseja processar um formulário que não tenha informações críticas.</span><span class="sxs-lookup"><span data-stu-id="5c614-125">For example, you don't want to process a form that's missing critical information.</span></span>

<span data-ttu-id="5c614-126">Quando os usuários inserem valores em um formulário HTML, os valores que eles inserem são cadeias de caracteres.</span><span class="sxs-lookup"><span data-stu-id="5c614-126">When users enter values into an HTML form, the values that they enter are strings.</span></span> <span data-ttu-id="5c614-127">Em muitos casos, os valores necessários são alguns outros tipos de dados, como números inteiros ou datas.</span><span class="sxs-lookup"><span data-stu-id="5c614-127">In many cases, the values you need are some other data types, like integers or dates.</span></span> <span data-ttu-id="5c614-128">Portanto, você também precisa certificar-se de que os valores inseridos pelos usuários possam ser convertidos corretamente nos tipos de dados apropriados.</span><span class="sxs-lookup"><span data-stu-id="5c614-128">Therefore, you also have to make sure that the values that users enter can be correctly converted to the appropriate data types.</span></span>

<span data-ttu-id="5c614-129">Você também pode ter certas restrições sobre os valores.</span><span class="sxs-lookup"><span data-stu-id="5c614-129">You might also have certain restrictions on the values.</span></span> <span data-ttu-id="5c614-130">Mesmo que os usuários insiram corretamente um inteiro, por exemplo, talvez seja necessário certificar-se de que o valor esteja dentro de um determinado intervalo.</span><span class="sxs-lookup"><span data-stu-id="5c614-130">Even if users correctly enter an integer, for example, you might need to make sure that the value falls within a certain range.</span></span>

![Erros de validação que usam classes de estilo CSS](validating-user-input-in-aspnet-web-pages-sites/_static/image1.png)

> [!NOTE] 
> 
> <span data-ttu-id="5c614-132">**Importante** A validação da entrada do usuário também é importante para segurança.</span><span class="sxs-lookup"><span data-stu-id="5c614-132">**Important** Validating user input is also important for security.</span></span> <span data-ttu-id="5c614-133">Ao restringir os valores que os usuários podem inserir em formulários, você reduz a chance de que alguém possa inserir um valor que possa comprometer a segurança do seu site.</span><span class="sxs-lookup"><span data-stu-id="5c614-133">When you restrict the values that users can enter in forms, you reduce the chance that someone can enter a value that can compromise the security of your site.</span></span>

<a id="Validating_User_Input"></a>
## <a name="validating-user-input"></a><span data-ttu-id="5c614-134">Validando entradas de usuário</span><span class="sxs-lookup"><span data-stu-id="5c614-134">Validating User Input</span></span>

<span data-ttu-id="5c614-135">No Páginas da Web do ASP.NET 2, você pode usar o auxiliar de `Validator` para testar a entrada do usuário.</span><span class="sxs-lookup"><span data-stu-id="5c614-135">In ASP.NET Web Pages 2, you can use the `Validator` helper to test user input.</span></span> <span data-ttu-id="5c614-136">A abordagem básica é fazer o seguinte:</span><span class="sxs-lookup"><span data-stu-id="5c614-136">The basic approach is to do the following:</span></span>

1. <span data-ttu-id="5c614-137">Determine quais elementos de entrada (campos) você deseja validar.</span><span class="sxs-lookup"><span data-stu-id="5c614-137">Determine which input elements (fields) you want to validate.</span></span>

    <span data-ttu-id="5c614-138">Normalmente, você valida valores em elementos `<input>` em um formulário.</span><span class="sxs-lookup"><span data-stu-id="5c614-138">You typically validate values in `<input>` elements in a form.</span></span> <span data-ttu-id="5c614-139">No entanto, é uma boa prática validar todas as entradas, até mesmo a entrada que vem de um elemento restrito, como uma lista de `<select>`.</span><span class="sxs-lookup"><span data-stu-id="5c614-139">However, it's a good practice to validate all input, even input that comes from a constrained element like a `<select>` list.</span></span> <span data-ttu-id="5c614-140">Isso ajuda a garantir que os usuários não ignorem os controles em uma página e enviem um formulário.</span><span class="sxs-lookup"><span data-stu-id="5c614-140">This helps to make sure that users don't bypass the controls on a page and submit a form.</span></span>
2. <span data-ttu-id="5c614-141">No código de página, adicione verificações de validação individuais para cada elemento de entrada usando métodos do auxiliar de `Validation`.</span><span class="sxs-lookup"><span data-stu-id="5c614-141">In the page code, add individual validation checks for each input element by using methods of the `Validation` helper.</span></span>

    <span data-ttu-id="5c614-142">Para verificar os campos obrigatórios, use `Validation.RequireField(field, [error message])` (para um campo individual) ou `Validation.RequireFields(field1, field2, ...))` (para obter uma lista de campos).</span><span class="sxs-lookup"><span data-stu-id="5c614-142">To check for required fields, use `Validation.RequireField(field, [error message])` (for an individual field) or `Validation.RequireFields(field1, field2, ...))` (for a list of fields).</span></span> <span data-ttu-id="5c614-143">Para outros tipos de validação, use `Validation.Add(field, ValidationType)`.</span><span class="sxs-lookup"><span data-stu-id="5c614-143">For other types of validation, use `Validation.Add(field, ValidationType)`.</span></span> <span data-ttu-id="5c614-144">Por `ValidationType`, você pode usar estas opções:</span><span class="sxs-lookup"><span data-stu-id="5c614-144">For `ValidationType`, you can use these options:</span></span>

    `Validator.DateTime ([error message])`  
   `Validator.Decimal([error message])`  
   `Validator.EqualsTo(otherField [, error message])`  
   `Validator.Float([error message])`  
   `Validator.Integer([error message])`  
   `Validator.Range(min, max [, error message])`  
   `Validator.RegEx(pattern [, error message])`  
   `Validator.Required([error message])`  
   `Validator.StringLength(length)`  
   `Validator.Url([error message])`
3. <span data-ttu-id="5c614-145">Quando a página for enviada, verifique se a validação foi aprovada verificando `Validation.IsValid`:</span><span class="sxs-lookup"><span data-stu-id="5c614-145">When the page is submitted, check whether validation has passed by checking `Validation.IsValid`:</span></span>

    [!code-csharp[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample1.cs)]

    <span data-ttu-id="5c614-146">Se houver erros de validação, você ignorará o processamento de página normal.</span><span class="sxs-lookup"><span data-stu-id="5c614-146">If there are any validation errors, you skip normal page processing.</span></span> <span data-ttu-id="5c614-147">Por exemplo, se a finalidade da página for atualizar um banco de dados, você não fará isso até que todos os erros de validação tenham sido corrigidos.</span><span class="sxs-lookup"><span data-stu-id="5c614-147">For example, if the purpose of the page is to update a database, you don't do that until all validation errors have been fixed.</span></span>
4. <span data-ttu-id="5c614-148">Se houver erros de validação, exiba as mensagens de erro na marcação da página usando `Html.ValidationSummary` ou `Html.ValidationMessage`ou ambos.</span><span class="sxs-lookup"><span data-stu-id="5c614-148">If there are validation errors, display error messages in the page's markup by using `Html.ValidationSummary` or `Html.ValidationMessage`, or both.</span></span>

<span data-ttu-id="5c614-149">O exemplo a seguir mostra uma página que ilustra essas etapas.</span><span class="sxs-lookup"><span data-stu-id="5c614-149">The following example shows a page that illustrates these steps.</span></span>

[!code-cshtml[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample2.cshtml)]

<span data-ttu-id="5c614-150">Para ver como a validação funciona, execute esta página e faça os erros deliberadamente.</span><span class="sxs-lookup"><span data-stu-id="5c614-150">To see how validation works, run this page and deliberately make mistakes.</span></span> <span data-ttu-id="5c614-151">Por exemplo, aqui está a aparência da página se você esquecer de inserir um nome de curso, se inserir um, e se você inserir uma data inválida:</span><span class="sxs-lookup"><span data-stu-id="5c614-151">For example, here's what the page looks like if you forget to enter a course name, if you enter an, and if you enter an invalid date:</span></span>

![Erros de validação na página renderizada](validating-user-input-in-aspnet-web-pages-sites/_static/image2.png)

<a id="Adding_Client-Side_Validation"></a>
## <a name="adding-client-side-validation"></a><span data-ttu-id="5c614-153">Adicionando validação do lado do cliente</span><span class="sxs-lookup"><span data-stu-id="5c614-153">Adding Client-Side Validation</span></span>

<span data-ttu-id="5c614-154">Por padrão, a entrada do usuário é validada depois que os usuários enviam a página, ou seja, a validação é executada no código do servidor.</span><span class="sxs-lookup"><span data-stu-id="5c614-154">By default, user input is validated after users submit the page — that is, the validation is performed in server code.</span></span> <span data-ttu-id="5c614-155">Uma desvantagem dessa abordagem é que os usuários não sabem que fizeram um erro até que eles enviem a página.</span><span class="sxs-lookup"><span data-stu-id="5c614-155">A disadvantage of this approach is that users don't know that they've made an error until after they submit the page.</span></span> <span data-ttu-id="5c614-156">Se um formulário for longo ou complexo, os erros de relatório somente depois que a página for enviada poderão ser inconvenientes para o usuário.</span><span class="sxs-lookup"><span data-stu-id="5c614-156">If a form is long or complex, reporting errors only after the page is submitted can be inconvenient to the user.</span></span>

<span data-ttu-id="5c614-157">Você pode adicionar suporte para executar a validação no script do cliente.</span><span class="sxs-lookup"><span data-stu-id="5c614-157">You can add support to perform validation in client script.</span></span> <span data-ttu-id="5c614-158">Nesse caso, a validação é executada à medida que os usuários trabalham no navegador.</span><span class="sxs-lookup"><span data-stu-id="5c614-158">In that case, the validation is performed as users work in the browser.</span></span> <span data-ttu-id="5c614-159">Por exemplo, suponha que você especifique que um valor deve ser um inteiro.</span><span class="sxs-lookup"><span data-stu-id="5c614-159">For example, suppose you specify that a value should be an integer.</span></span> <span data-ttu-id="5c614-160">Se um usuário inserir um valor não inteiro, o erro será relatado assim que o usuário sair do campo de entrada.</span><span class="sxs-lookup"><span data-stu-id="5c614-160">If a user enters a non-integer value, the error is reported as soon as the user leaves the entry field.</span></span> <span data-ttu-id="5c614-161">Os usuários obtêm comentários imediatos, o que é conveniente para eles.</span><span class="sxs-lookup"><span data-stu-id="5c614-161">Users get immediate feedback, which is convenient for them.</span></span> <span data-ttu-id="5c614-162">A validação baseada no cliente também pode reduzir o número de vezes que o usuário precisa enviar o formulário para corrigir vários erros.</span><span class="sxs-lookup"><span data-stu-id="5c614-162">Client-based validation can also reduce the number of times that the user has to submit the form to correct multiple errors.</span></span>

> [!NOTE]
> <span data-ttu-id="5c614-163">Mesmo se você usar a validação do lado do cliente, a validação sempre será executada no código do servidor.</span><span class="sxs-lookup"><span data-stu-id="5c614-163">Even if you use client-side validation, validation is always also performed in server code.</span></span> <span data-ttu-id="5c614-164">Executar a validação no código do servidor é uma medida de segurança, caso os usuários ignorem a validação baseada no cliente.</span><span class="sxs-lookup"><span data-stu-id="5c614-164">Performing validation in server code is a security measure, in case users bypass client-based validation.</span></span>

1. <span data-ttu-id="5c614-165">Registre as seguintes bibliotecas JavaScript na página:</span><span class="sxs-lookup"><span data-stu-id="5c614-165">Register the following JavaScript libraries in the page:</span></span>  

    [!code-html[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample3.html)]

   <span data-ttu-id="5c614-166">Duas das bibliotecas são carregáveis de uma CDN (rede de distribuição de conteúdo), de modo que você não precisa necessariamente tê-las em seu computador ou servidor.</span><span class="sxs-lookup"><span data-stu-id="5c614-166">Two of the libraries are loadable from a content delivery network (CDN), so you don't necessarily have to have them on your computer or server.</span></span> <span data-ttu-id="5c614-167">No entanto, você deve ter uma cópia local de *jQuery. Validate. uninvasive. js*.</span><span class="sxs-lookup"><span data-stu-id="5c614-167">However, you must have a local copy of *jquery.validate.unobtrusive.js*.</span></span> <span data-ttu-id="5c614-168">Se você ainda não estiver trabalhando com um modelo do WebMatrix (como o **site inicial** ) que inclui a biblioteca, crie um site de páginas da Web baseado no **site inicial**.</span><span class="sxs-lookup"><span data-stu-id="5c614-168">If you are not already working with a WebMatrix template (like **Starter Site** ) that includes the library, create a Web Pages site that's based on **Starter Site**.</span></span> <span data-ttu-id="5c614-169">Em seguida, copie o arquivo *. js* para seu site atual.</span><span class="sxs-lookup"><span data-stu-id="5c614-169">Then copy the *.js* file to your current site.</span></span>
2. <span data-ttu-id="5c614-170">Em marcação, para cada elemento que você está validando, adicione uma chamada para `Validation.For(field)`.</span><span class="sxs-lookup"><span data-stu-id="5c614-170">In markup, for each element that you're validating, add a call to `Validation.For(field)`.</span></span> <span data-ttu-id="5c614-171">Esse método emite atributos que são usados pela validação do lado do cliente.</span><span class="sxs-lookup"><span data-stu-id="5c614-171">This method emits attributes that are used by client-side validation.</span></span> <span data-ttu-id="5c614-172">(Em vez de emitir o código JavaScript real, o método emite atributos como `data-val-...`.</span><span class="sxs-lookup"><span data-stu-id="5c614-172">(Rather than emitting actual JavaScript code, the method emits attributes like `data-val-...`.</span></span> <span data-ttu-id="5c614-173">Esses atributos dão suporte à validação de cliente discreta que usa jQuery para fazer o trabalho.)</span><span class="sxs-lookup"><span data-stu-id="5c614-173">These attributes support unobtrusive client validation that uses jQuery to do the work.)</span></span>

<span data-ttu-id="5c614-174">A página a seguir mostra como adicionar recursos de validação do cliente ao exemplo mostrado anteriormente.</span><span class="sxs-lookup"><span data-stu-id="5c614-174">The following page shows how to add client validation features to the example shown earlier.</span></span>

[!code-cshtml[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample4.cshtml?highlight=35-39,51,61,71)]

<span data-ttu-id="5c614-175">Nem todas as verificações de validação são executadas no cliente.</span><span class="sxs-lookup"><span data-stu-id="5c614-175">Not all validation checks run on the client.</span></span> <span data-ttu-id="5c614-176">Em particular, a validação do tipo de dados (inteiro, data e assim por diante) não é executada no cliente.</span><span class="sxs-lookup"><span data-stu-id="5c614-176">In particular, data-type validation (integer, date, and so on) don't run on the client.</span></span> <span data-ttu-id="5c614-177">As verificações a seguir funcionam no cliente e no servidor:</span><span class="sxs-lookup"><span data-stu-id="5c614-177">The following checks work on both the client and server:</span></span>

- `Required`
- `Range(minValue, maxValue)`
- `StringLength(maxLength[, minLength])`
- `Regex(pattern)`
- `EqualsTo(otherField)`

<span data-ttu-id="5c614-178">Neste exemplo, o teste para uma data válida não funcionará no código do cliente.</span><span class="sxs-lookup"><span data-stu-id="5c614-178">In this example, the test for a valid date won't work in client code.</span></span> <span data-ttu-id="5c614-179">No entanto, o teste será executado no código do servidor.</span><span class="sxs-lookup"><span data-stu-id="5c614-179">However, the test will be performed in server code.</span></span>

<a id="Formatting_Validation_Errors"></a>
## <a name="formatting-validation-errors"></a><span data-ttu-id="5c614-180">Erros de validação de formatação</span><span class="sxs-lookup"><span data-stu-id="5c614-180">Formatting Validation Errors</span></span>

<span data-ttu-id="5c614-181">Você pode controlar como os erros de validação são exibidos definindo as classes CSS que têm os seguintes nomes reservados:</span><span class="sxs-lookup"><span data-stu-id="5c614-181">You can control how validation errors are displayed by defining CSS classes that have the following reserved names:</span></span>

- <span data-ttu-id="5c614-182">`field-validation-error`.</span><span class="sxs-lookup"><span data-stu-id="5c614-182">`field-validation-error`.</span></span> <span data-ttu-id="5c614-183">Define a saída do método `Html.ValidationMessage` quando ele está exibindo um erro.</span><span class="sxs-lookup"><span data-stu-id="5c614-183">Defines the output of the `Html.ValidationMessage` method when it's displaying an error.</span></span>
- <span data-ttu-id="5c614-184">`field-validation-valid`.</span><span class="sxs-lookup"><span data-stu-id="5c614-184">`field-validation-valid`.</span></span> <span data-ttu-id="5c614-185">Define a saída do método `Html.ValidationMessage` quando não há nenhum erro.</span><span class="sxs-lookup"><span data-stu-id="5c614-185">Defines the output of the `Html.ValidationMessage` method when there is no error.</span></span>
- <span data-ttu-id="5c614-186">`input-validation-error`.</span><span class="sxs-lookup"><span data-stu-id="5c614-186">`input-validation-error`.</span></span> <span data-ttu-id="5c614-187">Define como os elementos de `<input>` são renderizados quando há um erro.</span><span class="sxs-lookup"><span data-stu-id="5c614-187">Defines how `<input>` elements are rendered when there's an error.</span></span> <span data-ttu-id="5c614-188">(Por exemplo, você pode usar essa classe para definir a cor do plano de fundo de um &lt;entrada&gt; elemento para uma cor diferente se seu valor for inválido.) Essa classe CSS é usada somente durante a validação do cliente (no Páginas da Web do ASP.NET 2).</span><span class="sxs-lookup"><span data-stu-id="5c614-188">(For example, you can use this class to set the background color of an &lt;input&gt; element to a different color if its value is invalid.) This CSS class is used only during client validation (in ASP.NET Web Pages 2).</span></span>
- <span data-ttu-id="5c614-189">`input-validation-valid`.</span><span class="sxs-lookup"><span data-stu-id="5c614-189">`input-validation-valid`.</span></span> <span data-ttu-id="5c614-190">Define a aparência dos elementos de `<input>` quando não há nenhum erro.</span><span class="sxs-lookup"><span data-stu-id="5c614-190">Defines the appearance of `<input>` elements when there is no error.</span></span>
- <span data-ttu-id="5c614-191">`validation-summary-errors`.</span><span class="sxs-lookup"><span data-stu-id="5c614-191">`validation-summary-errors`.</span></span> <span data-ttu-id="5c614-192">Define a saída do método `Html.ValidationSummary` que está exibindo uma lista de erros.</span><span class="sxs-lookup"><span data-stu-id="5c614-192">Defines the output of the `Html.ValidationSummary` method it's displaying a list of errors.</span></span>
- <span data-ttu-id="5c614-193">`validation-summary-valid`.</span><span class="sxs-lookup"><span data-stu-id="5c614-193">`validation-summary-valid`.</span></span> <span data-ttu-id="5c614-194">Define a saída do método `Html.ValidationSummary` quando não há nenhum erro.</span><span class="sxs-lookup"><span data-stu-id="5c614-194">Defines the output of the `Html.ValidationSummary` method when there is no error.</span></span>

<span data-ttu-id="5c614-195">O bloco de `<style>` a seguir mostra regras para condições de erro.</span><span class="sxs-lookup"><span data-stu-id="5c614-195">The following `<style>` block shows rules for error conditions.</span></span>

[!code-css[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample5.css)]

<span data-ttu-id="5c614-196">Se você incluir esse bloco de estilo nas páginas de exemplo anteriores no artigo, a exibição do erro será parecida com a ilustração a seguir:</span><span class="sxs-lookup"><span data-stu-id="5c614-196">If you include this style block in the example pages from earlier in the article, the error display will look like the following illustration:</span></span>

![Erros de validação que usam classes de estilo CSS](validating-user-input-in-aspnet-web-pages-sites/_static/image3.png)

> [!NOTE]
> <span data-ttu-id="5c614-198">Se você não estiver usando a validação de cliente no Páginas da Web do ASP.NET 2, as classes CSS para os elementos `<input>` (`input-validation-error` e `input-validation-valid` não terão nenhum efeito.</span><span class="sxs-lookup"><span data-stu-id="5c614-198">If you're not using client validation in ASP.NET Web Pages 2, the CSS classes for the `<input>` elements (`input-validation-error` and `input-validation-valid` don't have any effect.</span></span>

### <a name="static-and-dynamic-error-display"></a><span data-ttu-id="5c614-199">Exibição de erro estático e dinâmico</span><span class="sxs-lookup"><span data-stu-id="5c614-199">Static and Dynamic Error Display</span></span>

<span data-ttu-id="5c614-200">As regras de CSS são fornecidas em pares, como `validation-summary-errors` e `validation-summary-valid`.</span><span class="sxs-lookup"><span data-stu-id="5c614-200">The CSS rules come in pairs, such as `validation-summary-errors` and `validation-summary-valid`.</span></span> <span data-ttu-id="5c614-201">Esses pares permitem definir regras para as duas condições: uma condição de erro e uma condição "normal" (não erro).</span><span class="sxs-lookup"><span data-stu-id="5c614-201">These pairs let you define rules for both conditions: an error condition and a "normal" (non-error) condition.</span></span> <span data-ttu-id="5c614-202">É importante entender que a marcação da exibição de erro sempre é renderizada, mesmo se não houver erros.</span><span class="sxs-lookup"><span data-stu-id="5c614-202">It's important to understand that the markup for the error display is always rendered, even if there are no errors.</span></span> <span data-ttu-id="5c614-203">Por exemplo, se uma página tiver um método `Html.ValidationSummary` na marcação, a origem da página conterá a marcação a seguir mesmo quando a página for solicitada pela primeira vez:</span><span class="sxs-lookup"><span data-stu-id="5c614-203">For example, if a page has an `Html.ValidationSummary` method in the markup, the page source will contain the following markup even when the page is requested for the first time:</span></span>

`<div class="validation-summary-valid" data-valmsg-summary="true"><ul></ul></div>`

<span data-ttu-id="5c614-204">Em outras palavras, o método `Html.ValidationSummary` sempre renderiza um elemento `<div>` e uma lista, mesmo que a lista de erros esteja vazia.</span><span class="sxs-lookup"><span data-stu-id="5c614-204">In other words, the `Html.ValidationSummary` method always renders a `<div>` element and a list, even if the error list is empty.</span></span> <span data-ttu-id="5c614-205">Da mesma forma, o método `Html.ValidationMessage` sempre renderiza um elemento `<span>` como um espaço reservado para um erro de campo individual, mesmo que não haja nenhum erro.</span><span class="sxs-lookup"><span data-stu-id="5c614-205">Similarly, the `Html.ValidationMessage` method always renders a `<span>` element as a placeholder for an individual field error, even if there is no error.</span></span>

<span data-ttu-id="5c614-206">Em algumas situações, a exibição de uma mensagem de erro pode fazer com que a página flua novamente e pode fazer com que elementos na página se movimentem.</span><span class="sxs-lookup"><span data-stu-id="5c614-206">In some situations, displaying an error message can cause the page to reflow and can cause elements on the page to move around.</span></span> <span data-ttu-id="5c614-207">As regras de CSS que terminam em `-valid` permitem que você defina um layout que pode ajudar a evitar esse problema.</span><span class="sxs-lookup"><span data-stu-id="5c614-207">The CSS rules that end in `-valid` let you define a layout that can help prevent this problem.</span></span> <span data-ttu-id="5c614-208">Por exemplo, você pode definir `field-validation-error` e `field-validation-valid` ter o mesmo tamanho fixo.</span><span class="sxs-lookup"><span data-stu-id="5c614-208">For example, you can define `field-validation-error` and `field-validation-valid` to both have the same fixed size.</span></span> <span data-ttu-id="5c614-209">Dessa forma, a área de exibição do campo é estática e não alterará o fluxo de página se uma mensagem de erro for exibida.</span><span class="sxs-lookup"><span data-stu-id="5c614-209">That way, the display area for the field is static and won't change the page flow if an error message is displayed.</span></span>

<a id="Validating_Data_That_Doesnt_Come_Directly_from_Users"></a>
## <a name="validating-data-that-doesnt-come-directly-from-users"></a><span data-ttu-id="5c614-210">Validando dados que não são fornecidos diretamente dos usuários</span><span class="sxs-lookup"><span data-stu-id="5c614-210">Validating Data That Doesn't Come Directly from Users</span></span>

<span data-ttu-id="5c614-211">Às vezes, você precisa validar as informações que não são fornecidas diretamente de um formulário HTML.</span><span class="sxs-lookup"><span data-stu-id="5c614-211">Sometimes you have to validate information that doesn't come directly from an HTML form.</span></span> <span data-ttu-id="5c614-212">Um exemplo típico é uma página em que um valor é passado em uma cadeia de caracteres de consulta, como no exemplo a seguir:</span><span class="sxs-lookup"><span data-stu-id="5c614-212">A typical example is a page where a value is passed in a query string, as in the following example:</span></span>

`http://server/myapp/EditClassInformation?classid=1022`

<span data-ttu-id="5c614-213">Nesse caso, você deseja certificar-se de que o valor passado para a página (aqui, 1022 para o valor de `classid`) seja válido.</span><span class="sxs-lookup"><span data-stu-id="5c614-213">In this case, you want to make sure that the value that's passed to the page (here, 1022 for the value of `classid`) is valid.</span></span> <span data-ttu-id="5c614-214">Você não pode usar diretamente o auxiliar de `Validation` para executar essa validação.</span><span class="sxs-lookup"><span data-stu-id="5c614-214">You can't directly use the `Validation` helper to perform this validation.</span></span> <span data-ttu-id="5c614-215">No entanto, você pode usar outros recursos do sistema de validação, como a capacidade de exibir mensagens de erro de validação.</span><span class="sxs-lookup"><span data-stu-id="5c614-215">However, you can use other features of the validation system, like the ability to display validation error messages.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="5c614-216">**Importante** Sempre valide os valores obtidos de *qualquer* fonte, incluindo valores de campo de formulário, valores de cadeia de caracteres de consulta e valores de cookie.</span><span class="sxs-lookup"><span data-stu-id="5c614-216">**Important** Always validate values that you get from *any* source, including form-field values, query-string values, and cookie values.</span></span> <span data-ttu-id="5c614-217">É fácil para as pessoas alterar esses valores (talvez para fins mal-intencionados).</span><span class="sxs-lookup"><span data-stu-id="5c614-217">It's easy for people to change these values (perhaps for malicious purposes).</span></span> <span data-ttu-id="5c614-218">Portanto, você deve verificar esses valores para proteger seu aplicativo.</span><span class="sxs-lookup"><span data-stu-id="5c614-218">So you must check these values in order to protect your application.</span></span>

<span data-ttu-id="5c614-219">O exemplo a seguir mostra como você pode validar um valor que é passado em uma cadeia de caracteres de consulta.</span><span class="sxs-lookup"><span data-stu-id="5c614-219">The following example shows how you might validate a value that's passed in a query string.</span></span> <span data-ttu-id="5c614-220">O código testa se o valor não está vazio e se é um inteiro.</span><span class="sxs-lookup"><span data-stu-id="5c614-220">The code tests that the value is not empty and that it's an integer.</span></span>

[!code-csharp[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample6.cs)]

<span data-ttu-id="5c614-221">Observe que o teste é executado quando a solicitação não é um envio de formulário (`if(!IsPost)`).</span><span class="sxs-lookup"><span data-stu-id="5c614-221">Notice that the test is performed when the request is not a form submission (`if(!IsPost)`).</span></span> <span data-ttu-id="5c614-222">Esse teste será aprovado na primeira vez que a página for solicitada, mas não quando a solicitação for um envio de formulário.</span><span class="sxs-lookup"><span data-stu-id="5c614-222">This test would pass the first time that the page is requested, but not when the request is a form submission.</span></span>

<span data-ttu-id="5c614-223">Para exibir esse erro, você pode adicionar o erro à lista de erros de validação chamando `Validation.AddFormError("message")`.</span><span class="sxs-lookup"><span data-stu-id="5c614-223">To display this error, you can add the error to the list of validation errors by calling `Validation.AddFormError("message")`.</span></span> <span data-ttu-id="5c614-224">Se a página contiver uma chamada para o método `Html.ValidationSummary`, o erro será exibido ali, assim como um erro de validação de entrada de usuário.</span><span class="sxs-lookup"><span data-stu-id="5c614-224">If the page contains a call to the `Html.ValidationSummary` method, the error is displayed there, just like a user-input validation error.</span></span>

<a id="AdditionalResources"></a>
## <a name="additional-resources"></a><span data-ttu-id="5c614-225">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="5c614-225">Additional Resources</span></span>

[<span data-ttu-id="5c614-226">Trabalhando com formulários HTML em sites Páginas da Web do ASP.NET</span><span class="sxs-lookup"><span data-stu-id="5c614-226">Working with HTML Forms in ASP.NET Web Pages Sites</span></span>](https://go.microsoft.com/fwlink/?LinkID=202892)
