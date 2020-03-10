---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/display_data_items_and_details
title: Exibir itens de dados e detalhes | Microsoft Docs
author: Erikre
description: Esta série de tutoriais mostrará as noções básicas da criação de um aplicativo ASP.NET Web Forms com ASP.NET 4,7 e Microsoft Visual Studio 2017
ms.author: riande
ms.date: 1/04/2019
ms.assetid: 64a491a8-0ed6-4c2f-9c1c-412962eb6006
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/display_data_items_and_details
msc.type: authoredcontent
ms.openlocfilehash: 130c9ffd29df612dac5bb954830a2eb9b738aaf0
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78641109"
---
# <a name="display-data-items-and-details"></a><span data-ttu-id="ddf14-103">Exibir itens de dados e detalhes</span><span class="sxs-lookup"><span data-stu-id="ddf14-103">Display data items and details</span></span>

<span data-ttu-id="ddf14-104">por [Erik Reitan](https://github.com/Erikre)</span><span class="sxs-lookup"><span data-stu-id="ddf14-104">by [Erik Reitan](https://github.com/Erikre)</span></span>

> <span data-ttu-id="ddf14-105">Esta série de tutoriais ensina as noções básicas da criação de um aplicativo ASP.NET Web Forms com ASP.NET 4,7 e Microsoft Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="ddf14-105">This tutorial series teaches you the basics of building an ASP.NET Web Forms application with ASP.NET 4.7 and Microsoft Visual Studio 2017.</span></span>

<span data-ttu-id="ddf14-106">Neste tutorial, você aprenderá a exibir itens de dados e detalhes de item de dados com ASP.NET Web Forms e Entity Framework Code First.</span><span class="sxs-lookup"><span data-stu-id="ddf14-106">In this tutorial, you'll learn how to display data items and data item details with ASP.NET Web Forms and Entity Framework Code First.</span></span> <span data-ttu-id="ddf14-107">Este tutorial se baseia no tutorial de "interface do usuário e navegação" anterior como parte da série de tutoriais da loja Wingtip Toy.</span><span class="sxs-lookup"><span data-stu-id="ddf14-107">This tutorial builds on the previous "UI and Navigation" tutorial as part of the Wingtip Toy Store tutorial series.</span></span> <span data-ttu-id="ddf14-108">Depois de concluir este tutorial, você verá produtos na página *ProductList. aspx* e nos detalhes do produto na página *ProductDetails. aspx* .</span><span class="sxs-lookup"><span data-stu-id="ddf14-108">After completing this tutorial, you'll see products on the *ProductsList.aspx* page and a product's details on the *ProductDetails.aspx* page.</span></span>

## <a name="youll-learn-how-to"></a><span data-ttu-id="ddf14-109">Você aprenderá a:</span><span class="sxs-lookup"><span data-stu-id="ddf14-109">You'll learn how to:</span></span>

- <span data-ttu-id="ddf14-110">Adicionar um controle de dados para exibir produtos do banco de dado</span><span class="sxs-lookup"><span data-stu-id="ddf14-110">Add a data control to display products from the database</span></span>
- <span data-ttu-id="ddf14-111">Conectar um controle de dados aos dados selecionados</span><span class="sxs-lookup"><span data-stu-id="ddf14-111">Connect a data control to the selected data</span></span>
- <span data-ttu-id="ddf14-112">Adicionar um controle de dados para exibir os detalhes do produto</span><span class="sxs-lookup"><span data-stu-id="ddf14-112">Add a data control to display product details from the database</span></span>
- <span data-ttu-id="ddf14-113">Recuperar um valor da cadeia de caracteres de consulta e usar esse valor para limitar os dados que são recuperados do Database</span><span class="sxs-lookup"><span data-stu-id="ddf14-113">Retrieve a value from the query string and use that value to limit the data that's retrieved from the database</span></span>

### <a name="features-introduced-in-this-tutorial"></a><span data-ttu-id="ddf14-114">Recursos apresentados neste tutorial:</span><span class="sxs-lookup"><span data-stu-id="ddf14-114">Features introduced in this tutorial:</span></span>

- <span data-ttu-id="ddf14-115">Model binding</span><span class="sxs-lookup"><span data-stu-id="ddf14-115">Model binding</span></span>
- <span data-ttu-id="ddf14-116">Provedores de valor</span><span class="sxs-lookup"><span data-stu-id="ddf14-116">Value providers</span></span>

## <a name="add-a-data-control"></a><span data-ttu-id="ddf14-117">Adicionar um controle de dados</span><span class="sxs-lookup"><span data-stu-id="ddf14-117">Add a data control</span></span>

<span data-ttu-id="ddf14-118">Você pode usar algumas opções diferentes para associar dados a um controle de servidor.</span><span class="sxs-lookup"><span data-stu-id="ddf14-118">You can use a few different options to bind data to a server control.</span></span> <span data-ttu-id="ddf14-119">As mais comuns incluem:</span><span class="sxs-lookup"><span data-stu-id="ddf14-119">The most common include:</span></span>

* <span data-ttu-id="ddf14-120">Adicionando um controle da fonte de dados</span><span class="sxs-lookup"><span data-stu-id="ddf14-120">Adding a data source control</span></span>
* <span data-ttu-id="ddf14-121">Adicionando código manualmente</span><span class="sxs-lookup"><span data-stu-id="ddf14-121">Adding code by hand</span></span>
* <span data-ttu-id="ddf14-122">Usando a associação de modelo</span><span class="sxs-lookup"><span data-stu-id="ddf14-122">Using model binding</span></span>

### <a name="use-a-data-source-control-to-bind-data"></a><span data-ttu-id="ddf14-123">Usar um controle de fonte de dados para associar dados</span><span class="sxs-lookup"><span data-stu-id="ddf14-123">Use a data source control to bind data</span></span>

<span data-ttu-id="ddf14-124">Adicionar um controle de fonte de dados permite vincular o controle da fonte de dados ao controle que exibe os dados.</span><span class="sxs-lookup"><span data-stu-id="ddf14-124">Adding a data source control allows you to link the data source control to the control that displays the data.</span></span> <span data-ttu-id="ddf14-125">Com essa abordagem, você pode declarativamente, em vez de programaticamente, conectar controles do lado do servidor a fontes de dados.</span><span class="sxs-lookup"><span data-stu-id="ddf14-125">With this approach, you can declaratively,  rather than programmatically, connect server-side controls to data sources.</span></span>

### <a name="code-by-hand-to-bind-data"></a><span data-ttu-id="ddf14-126">Código manualmente para associar dados</span><span class="sxs-lookup"><span data-stu-id="ddf14-126">Code by hand to bind data</span></span>

<span data-ttu-id="ddf14-127">Codificar manualmente envolve:</span><span class="sxs-lookup"><span data-stu-id="ddf14-127">Coding by hand involves:</span></span>

1. <span data-ttu-id="ddf14-128">Lendo um valor</span><span class="sxs-lookup"><span data-stu-id="ddf14-128">Reading a value</span></span>
2. <span data-ttu-id="ddf14-129">Verificando se é nulo</span><span class="sxs-lookup"><span data-stu-id="ddf14-129">Checking if it's null</span></span>
3. <span data-ttu-id="ddf14-130">Convertendo-o para um tipo apropriado</span><span class="sxs-lookup"><span data-stu-id="ddf14-130">Converting it to an appropriate type</span></span>
4. <span data-ttu-id="ddf14-131">Verificando o êxito da conversão</span><span class="sxs-lookup"><span data-stu-id="ddf14-131">Checking conversion success</span></span>
5. <span data-ttu-id="ddf14-132">Usando o valor na consulta</span><span class="sxs-lookup"><span data-stu-id="ddf14-132">Using the value in the query</span></span> 

<span data-ttu-id="ddf14-133">Essa abordagem permite que você tenha controle total sobre a lógica de acesso a dados.</span><span class="sxs-lookup"><span data-stu-id="ddf14-133">This approach lets you have full control over your data-access logic.</span></span>

### <a name="use-model-binding-to-bind-data"></a><span data-ttu-id="ddf14-134">Usar Associação de modelo para associar dados</span><span class="sxs-lookup"><span data-stu-id="ddf14-134">Use model binding to bind data</span></span>

<span data-ttu-id="ddf14-135">A associação de modelo permite que você associe os resultados com muito menos código e oferece a capacidade de reutilizar a funcionalidade em todo o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="ddf14-135">Model binding lets you bind results with far less code and gives you the ability to reuse the functionality throughout your application.</span></span> <span data-ttu-id="ddf14-136">Ele simplifica o trabalho com a lógica de acesso a dados focada em código enquanto ainda fornece uma estrutura de ligação de dados rica.</span><span class="sxs-lookup"><span data-stu-id="ddf14-136">It simplifies working with code-focused data-access logic while still providing a rich, data-binding framework.</span></span>

## <a name="display-products"></a><span data-ttu-id="ddf14-137">Exibir produtos</span><span class="sxs-lookup"><span data-stu-id="ddf14-137">Display products</span></span>

<span data-ttu-id="ddf14-138">Neste tutorial, você usará a associação de modelo para associar dados.</span><span class="sxs-lookup"><span data-stu-id="ddf14-138">In this tutorial, you'll use model binding to bind data.</span></span> <span data-ttu-id="ddf14-139">Para configurar um controle de dados para usar a associação de modelo para selecionar dados, defina a propriedade `SelectMethod` do controle como um nome de método no código da página.</span><span class="sxs-lookup"><span data-stu-id="ddf14-139">To configure a data control to use model binding to select data, you set the control's `SelectMethod` property to a method name in the page's code.</span></span> <span data-ttu-id="ddf14-140">O controle de dados chama o método no momento apropriado no ciclo de vida da página e associa os dados retornados automaticamente.</span><span class="sxs-lookup"><span data-stu-id="ddf14-140">The data control calls the method at the appropriate time in the page life cycle and automatically binds the returned data.</span></span> <span data-ttu-id="ddf14-141">Não é necessário chamar explicitamente o método `DataBind`.</span><span class="sxs-lookup"><span data-stu-id="ddf14-141">There's no need to explicitly call the `DataBind` method.</span></span>

1. <span data-ttu-id="ddf14-142">Em **Gerenciador de soluções**, abra *ProductList. aspx*.</span><span class="sxs-lookup"><span data-stu-id="ddf14-142">In **Solution Explorer**, open *ProductList.aspx*.</span></span>
2. <span data-ttu-id="ddf14-143">Substituir a marcação existente por esta marcação:</span><span class="sxs-lookup"><span data-stu-id="ddf14-143">Replace the existing markup with this markup:</span></span>   

    [!code-aspx-csharp[Main](display_data_items_and_details/samples/sample1.aspx)]

<span data-ttu-id="ddf14-144">Esse código usa um controle **ListView** chamado `productList` para exibir produtos.</span><span class="sxs-lookup"><span data-stu-id="ddf14-144">This code uses a **ListView** control named `productList` to display products.</span></span>

[!code-aspx-csharp[Main](display_data_items_and_details/samples/sample2.aspx)]

<span data-ttu-id="ddf14-145">Com modelos e estilos, você define como o controle **ListView** exibe dados.</span><span class="sxs-lookup"><span data-stu-id="ddf14-145">With templates and styles, you define how the **ListView** control displays data.</span></span> <span data-ttu-id="ddf14-146">Ele é útil para dados em qualquer estrutura repetitiva.</span><span class="sxs-lookup"><span data-stu-id="ddf14-146">It's useful for data in any repeating structure.</span></span> <span data-ttu-id="ddf14-147">Embora este exemplo de **ListView** simplesmente exiba dados de banco de dado, você também pode, sem código, permitir que os usuários editem, insiram e excluam dados, e classifiquem e coloquem dados na página.</span><span class="sxs-lookup"><span data-stu-id="ddf14-147">Though this **ListView** example simply displays database data, you can also, without code, enable users to edit, insert, and delete data, and to sort and page data.</span></span>

<span data-ttu-id="ddf14-148">Ao definir a propriedade `ItemType` no controle **ListView** , a expressão de vinculação de dados `Item` está disponível e o controle se torna fortemente tipado.</span><span class="sxs-lookup"><span data-stu-id="ddf14-148">By setting the `ItemType` property in the **ListView** control, the data-binding expression `Item` is available and the control becomes strongly typed.</span></span> <span data-ttu-id="ddf14-149">Conforme mencionado no tutorial anterior, você pode selecionar detalhes de objeto de item com o IntelliSense, como especificar o `ProductName`:</span><span class="sxs-lookup"><span data-stu-id="ddf14-149">As mentioned in the previous tutorial, you can select Item object details with IntelliSense, such as specifying the `ProductName`:</span></span>

![Exibir itens de dados e detalhes-IntelliSense](display_data_items_and_details/_static/image1.png)

<span data-ttu-id="ddf14-151">Você também está usando a associação de modelo para especificar um valor de `SelectMethod`.</span><span class="sxs-lookup"><span data-stu-id="ddf14-151">You're also using model binding to specify a `SelectMethod` value.</span></span> <span data-ttu-id="ddf14-152">Esse valor (`GetProducts`) corresponde ao método que você adicionará ao code-behind para exibir produtos na próxima etapa.</span><span class="sxs-lookup"><span data-stu-id="ddf14-152">This value (`GetProducts`) corresponds to the method you'll add to the code behind to display products in the next step.</span></span>

### <a name="add-code-to-display-products"></a><span data-ttu-id="ddf14-153">Adicionar código para exibir produtos</span><span class="sxs-lookup"><span data-stu-id="ddf14-153">Add code to display products</span></span>

<span data-ttu-id="ddf14-154">Nesta etapa, você adicionará o código para preencher o controle **ListView** com os dados do produto do banco de dado.</span><span class="sxs-lookup"><span data-stu-id="ddf14-154">In this step, you'll add code to populate the **ListView** control with product data from the database.</span></span> <span data-ttu-id="ddf14-155">O código dá suporte à exibição de todos os produtos e produtos de categoria individuais.</span><span class="sxs-lookup"><span data-stu-id="ddf14-155">The code supports showing all products and  individual category products.</span></span>

1. <span data-ttu-id="ddf14-156">Em **Gerenciador de soluções**, clique com o botão direito do mouse em *ProductList. aspx* e selecione **Exibir código**.</span><span class="sxs-lookup"><span data-stu-id="ddf14-156">In **Solution Explorer**, right-click *ProductList.aspx* and then select **View Code**.</span></span>
2. <span data-ttu-id="ddf14-157">Substitua o código existente no arquivo *ProductList.aspx.cs* por:</span><span class="sxs-lookup"><span data-stu-id="ddf14-157">Replace the existing code in the *ProductList.aspx.cs* file with this:</span></span>   

    [!code-csharp[Main](display_data_items_and_details/samples/sample3.cs)]

<span data-ttu-id="ddf14-158">Esse código mostra o método `GetProducts` que a propriedade `ItemType` do controle **ListView** faz referência na página *ProductList. aspx* .</span><span class="sxs-lookup"><span data-stu-id="ddf14-158">This code shows the `GetProducts` method that the **ListView** control's `ItemType` property references in the *ProductList.aspx* page.</span></span> <span data-ttu-id="ddf14-159">Para limitar os resultados a uma categoria de banco de dados específica, o código define o valor `categoryId` do valor da cadeia de caracteres de consulta passado para a página *ProductList. aspx* quando a página *ProductList. aspx* é navegada.</span><span class="sxs-lookup"><span data-stu-id="ddf14-159">To limit the results to a specific database category, the code sets the `categoryId` value from the query string value passed to the *ProductList.aspx* page when the *ProductList.aspx* page is navigated to.</span></span> <span data-ttu-id="ddf14-160">A classe `QueryStringAttribute` no namespace `System.Web.ModelBinding` é usada para recuperar o valor da variável de cadeia de caracteres de consulta `id`.</span><span class="sxs-lookup"><span data-stu-id="ddf14-160">The `QueryStringAttribute` class in the `System.Web.ModelBinding` namespace is used to retrieve the value of the query string variable `id`.</span></span> <span data-ttu-id="ddf14-161">Isso instrui a associação de modelo para tentar associar um valor da cadeia de caracteres de consulta ao parâmetro `categoryId` em tempo de execução.</span><span class="sxs-lookup"><span data-stu-id="ddf14-161">This instructs model binding to try to bind a value from the query string to the `categoryId` parameter at run time.</span></span>

<span data-ttu-id="ddf14-162">Quando uma categoria válida é passada como uma cadeia de caracteres de consulta para a página, os resultados da consulta são limitados a esses produtos no banco de dados que correspondem ao valor de `categoryId`.</span><span class="sxs-lookup"><span data-stu-id="ddf14-162">When a valid category is passed as a query string to the page, the results of the query are limited to those products in the database that match the `categoryId` value.</span></span> <span data-ttu-id="ddf14-163">Por exemplo, se a URL da página *ProductList. aspx* for esta:</span><span class="sxs-lookup"><span data-stu-id="ddf14-163">For instance, if the *ProductsList.aspx* page URL is this:</span></span>

[!code-console[Main](display_data_items_and_details/samples/sample4.cmd)]

<span data-ttu-id="ddf14-164">A página exibe somente os produtos em que o `categoryId` é igual a `1`.</span><span class="sxs-lookup"><span data-stu-id="ddf14-164">The page displays only the products where the `categoryId` equals `1`.</span></span>

<span data-ttu-id="ddf14-165">Todos os produtos serão exibidos se nenhuma cadeia de caracteres de consulta for incluída quando a página *ProductList. aspx* for chamada.</span><span class="sxs-lookup"><span data-stu-id="ddf14-165">All products are displayed if no query string is included when the *ProductList.aspx* page is called.</span></span>

<span data-ttu-id="ddf14-166">As fontes de valores para esses métodos são chamadas de *provedores de valor* (como *QueryString*) e os atributos de parâmetro que indicam qual provedor de valor usar são chamados de atributos de *provedor de valor* (como `id`).</span><span class="sxs-lookup"><span data-stu-id="ddf14-166">The sources of values for these methods are referred to as *value providers* (such as *QueryString*), and the parameter attributes that indicate which value provider to use are referred to as *value provider attributes* (such as `id`).</span></span> <span data-ttu-id="ddf14-167">O ASP.NET inclui provedores de valor e atributos correspondentes para todas as fontes típicas de entrada do usuário em um aplicativo Web Forms como a cadeia de caracteres de consulta, cookies, valores de formulário, controles, estado de exibição, estado de sessão e propriedades de perfil.</span><span class="sxs-lookup"><span data-stu-id="ddf14-167">ASP.NET includes value providers and corresponding attributes for all of the typical sources of user input in a Web Forms application such as the query string, cookies, form values, controls, view state, session state, and profile properties.</span></span> <span data-ttu-id="ddf14-168">Você também pode escrever provedores de valor personalizado.</span><span class="sxs-lookup"><span data-stu-id="ddf14-168">You can also write custom value providers.</span></span>

### <a name="run-the-application"></a><span data-ttu-id="ddf14-169">Executar o aplicativo</span><span class="sxs-lookup"><span data-stu-id="ddf14-169">Run the application</span></span>

<span data-ttu-id="ddf14-170">Execute o aplicativo agora para exibir todos os produtos ou os produtos de uma categoria.</span><span class="sxs-lookup"><span data-stu-id="ddf14-170">Run the application now to view all products or a category's products.</span></span>

1. <span data-ttu-id="ddf14-171">Pressione **F5** enquanto estiver no Visual Studio para executar o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="ddf14-171">Press **F5** while in Visual Studio to run the application.</span></span>  
   <span data-ttu-id="ddf14-172">O navegador é aberto e mostra a página *Default. aspx* .</span><span class="sxs-lookup"><span data-stu-id="ddf14-172">The browser opens and shows the *Default.aspx* page.</span></span>

2. <span data-ttu-id="ddf14-173">Selecione **carros** no menu de navegação categoria do produto.</span><span class="sxs-lookup"><span data-stu-id="ddf14-173">Select **Cars** from the product category navigation menu.</span></span>  
   <span data-ttu-id="ddf14-174">A página *ProductList. aspx* exibe mostrando apenas os produtos da categoria **carros** .</span><span class="sxs-lookup"><span data-stu-id="ddf14-174">The *ProductList.aspx* page displays showing only **Cars** category products.</span></span> <span data-ttu-id="ddf14-175">Posteriormente neste tutorial, você exibirá os detalhes do produto.</span><span class="sxs-lookup"><span data-stu-id="ddf14-175">Later in this tutorial, you'll display product details.</span></span>  

    ![Exibir itens de dados e detalhes-carros](display_data_items_and_details/_static/image2.png)

3. <span data-ttu-id="ddf14-177">Selecione **produtos** no menu de navegação na parte superior.</span><span class="sxs-lookup"><span data-stu-id="ddf14-177">Select **Products** from the navigation menu at the top.</span></span>  
   <span data-ttu-id="ddf14-178">Novamente, a página *ProductList. aspx* é exibida, no entanto, desta vez ele mostra a lista completa de produtos.</span><span class="sxs-lookup"><span data-stu-id="ddf14-178">Again, the *ProductList.aspx* page is displayed, however this time it shows the entire list of products.</span></span>   

    ![Exibir itens de dados e detalhes-produtos](display_data_items_and_details/_static/image3.png)

4. <span data-ttu-id="ddf14-180">Feche o navegador e retorne ao Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="ddf14-180">Close the browser and return to Visual Studio.</span></span>

### <a name="add-a-data-control-to-display-product-details"></a><span data-ttu-id="ddf14-181">Adicionar um controle de dados para exibir detalhes do produto</span><span class="sxs-lookup"><span data-stu-id="ddf14-181">Add a data control to display product details</span></span>

<span data-ttu-id="ddf14-182">Em seguida, você modificará a marcação na página *ProductDetails. aspx* que você adicionou no tutorial anterior para exibir informações específicas do produto.</span><span class="sxs-lookup"><span data-stu-id="ddf14-182">Next, you'll modify the markup in the *ProductDetails.aspx* page that you added in the previous tutorial to display specific product information.</span></span>

1. <span data-ttu-id="ddf14-183">No **Gerenciador de soluções**, abra *ProductDetails. aspx*.</span><span class="sxs-lookup"><span data-stu-id="ddf14-183">In **Solution Explorer**, open *ProductDetails.aspx*.</span></span>

2. <span data-ttu-id="ddf14-184">Substituir a marcação existente por esta marcação:</span><span class="sxs-lookup"><span data-stu-id="ddf14-184">Replace the existing markup with this markup:</span></span>

    [!code-aspx-csharp[Main](display_data_items_and_details/samples/sample5.aspx)] 

    <span data-ttu-id="ddf14-185">Esse código usa um controle **FormView** para exibir detalhes específicos do produto.</span><span class="sxs-lookup"><span data-stu-id="ddf14-185">This code uses a **FormView** control to display specific product details.</span></span> <span data-ttu-id="ddf14-186">Essa marcação usa métodos como os métodos usados para exibir dados na página *ProductList. aspx* .</span><span class="sxs-lookup"><span data-stu-id="ddf14-186">This markup uses methods like the methods used to display data in the *ProductList.aspx* page.</span></span> <span data-ttu-id="ddf14-187">O controle **FormView** é usado para exibir um único registro por vez de uma fonte de dados.</span><span class="sxs-lookup"><span data-stu-id="ddf14-187">The **FormView** control is used to display a single record at a time from a data source.</span></span> <span data-ttu-id="ddf14-188">Quando você usa o controle **FormView** , cria modelos para exibir e editar valores associados a dados.</span><span class="sxs-lookup"><span data-stu-id="ddf14-188">When you use the **FormView** control, you create templates to display and edit data-bound values.</span></span> <span data-ttu-id="ddf14-189">Esses modelos contêm controles, expressões de associação e formatação que definem a aparência e a funcionalidade do formulário.</span><span class="sxs-lookup"><span data-stu-id="ddf14-189">These templates contain controls, binding expressions, and formatting that define the form's look and functionality.</span></span>

<span data-ttu-id="ddf14-190">Conectar a marcação anterior ao banco de dados requer código adicional.</span><span class="sxs-lookup"><span data-stu-id="ddf14-190">Connecting the previous markup to the database requires additional code.</span></span>

1. <span data-ttu-id="ddf14-191">Em **Gerenciador de soluções**, clique com o botão direito do mouse em *ProductDetails. aspx* e clique em **Exibir código**.</span><span class="sxs-lookup"><span data-stu-id="ddf14-191">In **Solution Explorer**, right-click *ProductDetails.aspx* and then click **View Code**.</span></span>  
   <span data-ttu-id="ddf14-192">O arquivo *ProductDetails.aspx.cs* é exibido.</span><span class="sxs-lookup"><span data-stu-id="ddf14-192">The *ProductDetails.aspx.cs* file is displayed.</span></span>

2. <span data-ttu-id="ddf14-193">Substitua o código existente por este código:</span><span class="sxs-lookup"><span data-stu-id="ddf14-193">Replace the existing code with this code:</span></span>   

    [!code-csharp[Main](display_data_items_and_details/samples/sample6.cs)]

<span data-ttu-id="ddf14-194">Esse código verifica um valor de cadeia de caracteres de consulta "`productID`".</span><span class="sxs-lookup"><span data-stu-id="ddf14-194">This code checks for a "`productID`" query-string value.</span></span> <span data-ttu-id="ddf14-195">Se um valor de cadeia de caracteres de consulta válido for encontrado, o produto correspondente será exibido.</span><span class="sxs-lookup"><span data-stu-id="ddf14-195">If a valid query-string value is found, the matching product is displayed.</span></span> <span data-ttu-id="ddf14-196">Se a cadeia de caracteres de consulta não for encontrada ou seu valor não for válido, nenhum produto será exibido.</span><span class="sxs-lookup"><span data-stu-id="ddf14-196">If the query-string isn't found, or its value isn't valid, no product is displayed.</span></span>

### <a name="run-the-application"></a><span data-ttu-id="ddf14-197">Executar o aplicativo</span><span class="sxs-lookup"><span data-stu-id="ddf14-197">Run the application</span></span>

<span data-ttu-id="ddf14-198">Agora você pode executar o aplicativo para ver um produto individual exibido com base na ID do produto.</span><span class="sxs-lookup"><span data-stu-id="ddf14-198">Now you can run the application to see an individual product displayed based on product ID.</span></span>

1. <span data-ttu-id="ddf14-199">Pressione **F5** enquanto estiver no Visual Studio para executar o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="ddf14-199">Press **F5** while in Visual Studio to run the application.</span></span>  
   <span data-ttu-id="ddf14-200">O navegador é aberto e mostra a página *Default. aspx* .</span><span class="sxs-lookup"><span data-stu-id="ddf14-200">The browser opens and shows the *Default.aspx* page.</span></span>

2. <span data-ttu-id="ddf14-201">Selecione **Boats** no menu de navegação categoria.</span><span class="sxs-lookup"><span data-stu-id="ddf14-201">Select **Boats** from the category navigation menu.</span></span>  
   <span data-ttu-id="ddf14-202">A página *ProductList. aspx* é exibida.</span><span class="sxs-lookup"><span data-stu-id="ddf14-202">The *ProductList.aspx* page is displayed.</span></span>

3. <span data-ttu-id="ddf14-203">Selecione o **barco de papel** na lista de produtos.</span><span class="sxs-lookup"><span data-stu-id="ddf14-203">Select **Paper Boat** from the product list.</span></span>
   <span data-ttu-id="ddf14-204">A página *ProductDetails. aspx* é exibida.</span><span class="sxs-lookup"><span data-stu-id="ddf14-204">The *ProductDetails.aspx* page is displayed.</span></span>

    ![Exibir itens de dados e detalhes-produtos](display_data_items_and_details/_static/image4.png)
    
4. <span data-ttu-id="ddf14-206">Feche o navegador.</span><span class="sxs-lookup"><span data-stu-id="ddf14-206">Close the browser.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="ddf14-207">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="ddf14-207">Additional resources</span></span>

[<span data-ttu-id="ddf14-208">Recuperando e exibindo dados com associação de modelo e formulários da Web</span><span class="sxs-lookup"><span data-stu-id="ddf14-208">Retrieving and displaying data with model binding and web forms</span></span>](../../presenting-and-managing-data/model-binding/retrieving-data.md)

## <a name="next-steps"></a><span data-ttu-id="ddf14-209">Próximas etapas</span><span class="sxs-lookup"><span data-stu-id="ddf14-209">Next steps</span></span>

<span data-ttu-id="ddf14-210">Neste tutorial, você adicionou marcação e código para exibir produtos e detalhes do produto.</span><span class="sxs-lookup"><span data-stu-id="ddf14-210">In this tutorial, you added markup and code to display products and product details.</span></span> <span data-ttu-id="ddf14-211">Você aprendeu sobre controles de dados fortemente tipados, associação de modelo e provedores de valor.</span><span class="sxs-lookup"><span data-stu-id="ddf14-211">You learned about strongly typed data controls, model binding, and value providers.</span></span> <span data-ttu-id="ddf14-212">No próximo tutorial, você adicionará um carrinho de compras ao aplicativo de exemplo Wingtip Toys.</span><span class="sxs-lookup"><span data-stu-id="ddf14-212">In the next tutorial, you'll add a shopping cart to the Wingtip Toys sample application.</span></span> 

> [!div class="step-by-step"]
> <span data-ttu-id="ddf14-213">[Anterior](ui_and_navigation.md)
> [Próximo](shopping-cart.md)</span><span class="sxs-lookup"><span data-stu-id="ddf14-213">[Previous](ui_and_navigation.md)
[Next](shopping-cart.md)</span></span>
