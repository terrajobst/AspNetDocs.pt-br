---
uid: web-forms/overview/data-access/custom-formatting/using-the-formview-s-templates-cs
title: Usando os modelos de FormView (c#) | Microsoft Docs
author: rick-anderson
description: Ao contrário de DetailsView, FormView não é composto de campos. Em vez disso, FormView é renderizado usando modelos. Neste tutorial, examinaremos usando a F....
ms.author: riande
ms.date: 03/31/2010
ms.assetid: d3f062af-88cf-426d-af44-e41f32c41672
msc.legacyurl: /web-forms/overview/data-access/custom-formatting/using-the-formview-s-templates-cs
msc.type: authoredcontent
ms.openlocfilehash: 1a7cf17d8cbd0a5a17a387b9a70336a1b06efde7
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/01/2019
ms.locfileid: "57054993"
---
<a name="using-the-formviews-templates-c"></a><span data-ttu-id="9d413-105">Usando os modelos de FormView (c#)</span><span class="sxs-lookup"><span data-stu-id="9d413-105">Using the FormView's Templates (C#)</span></span>
====================
<span data-ttu-id="9d413-106">por [Scott Mitchell](https://twitter.com/ScottOnWriting)</span><span class="sxs-lookup"><span data-stu-id="9d413-106">by [Scott Mitchell](https://twitter.com/ScottOnWriting)</span></span>

<span data-ttu-id="9d413-107">[Baixe o aplicativo de exemplo](http://download.microsoft.com/download/9/6/9/969e5c94-dfb6-4e47-9570-d6d9e704c3c1/ASPNET_Data_Tutorial_14_CS.exe) ou [baixar PDF](using-the-formview-s-templates-cs/_static/datatutorial14cs1.pdf)</span><span class="sxs-lookup"><span data-stu-id="9d413-107">[Download Sample App](http://download.microsoft.com/download/9/6/9/969e5c94-dfb6-4e47-9570-d6d9e704c3c1/ASPNET_Data_Tutorial_14_CS.exe) or [Download PDF](using-the-formview-s-templates-cs/_static/datatutorial14cs1.pdf)</span></span>

> <span data-ttu-id="9d413-108">Ao contrário de DetailsView, FormView não é composto de campos.</span><span class="sxs-lookup"><span data-stu-id="9d413-108">Unlike the DetailsView, the FormView is not composed of fields.</span></span> <span data-ttu-id="9d413-109">Em vez disso, FormView é renderizado usando modelos.</span><span class="sxs-lookup"><span data-stu-id="9d413-109">Instead, the FormView is rendered using templates.</span></span> <span data-ttu-id="9d413-110">Neste tutorial, que vamos examinar usando o controle FormView para apresentar uma exibição menos rígida de dados.</span><span class="sxs-lookup"><span data-stu-id="9d413-110">In this tutorial we'll examine using the FormView control to present a less rigid display of data.</span></span>


## <a name="introduction"></a><span data-ttu-id="9d413-111">Introdução</span><span class="sxs-lookup"><span data-stu-id="9d413-111">Introduction</span></span>

<span data-ttu-id="9d413-112">Nos dois últimos tutoriais que vimos como personalizar as saídas dos controles GridView e DetailsView Usando TemplateFields.</span><span class="sxs-lookup"><span data-stu-id="9d413-112">In the last two tutorials we saw how to customize the GridView and DetailsView controls' outputs using TemplateFields.</span></span> <span data-ttu-id="9d413-113">TemplateFields permitir para o conteúdo para um campo específico ser altamente personalizados, mas no final o GridView e DetailsView têm uma aparência boxy em vez disso, como grade.</span><span class="sxs-lookup"><span data-stu-id="9d413-113">TemplateFields allow for the contents for a specific field to be highly customized, but in the end both the GridView and DetailsView have a rather boxy, grid-like appearance.</span></span> <span data-ttu-id="9d413-114">Para muitos cenários de tal um layout de grade é ideal, mas às vezes uma exibição mais fluida, menos rígida é necessária.</span><span class="sxs-lookup"><span data-stu-id="9d413-114">For many scenarios such a grid-like layout is ideal, but at times a more fluid, less rigid display is needed.</span></span> <span data-ttu-id="9d413-115">Ao exibir um único registro, tal um layout fluido é possível usar o controle FormView.</span><span class="sxs-lookup"><span data-stu-id="9d413-115">When displaying a single record, such a fluid layout is possible using the FormView control.</span></span>

<span data-ttu-id="9d413-116">Ao contrário de DetailsView, FormView não é composto de campos.</span><span class="sxs-lookup"><span data-stu-id="9d413-116">Unlike the DetailsView, the FormView is not composed of fields.</span></span> <span data-ttu-id="9d413-117">É possível adicionar um BoundField ou o TemplateField para um FormView.</span><span class="sxs-lookup"><span data-stu-id="9d413-117">You can't add a BoundField or TemplateField to a FormView.</span></span> <span data-ttu-id="9d413-118">Em vez disso, FormView é renderizado usando modelos.</span><span class="sxs-lookup"><span data-stu-id="9d413-118">Instead, the FormView is rendered using templates.</span></span> <span data-ttu-id="9d413-119">Pense FormView como um controle DetailsView que contém um TemplateField único.</span><span class="sxs-lookup"><span data-stu-id="9d413-119">Think of the FormView as a DetailsView control that contains a single TemplateField.</span></span> <span data-ttu-id="9d413-120">FormView suporta os seguintes modelos:</span><span class="sxs-lookup"><span data-stu-id="9d413-120">The FormView supports the following templates:</span></span>

- <span data-ttu-id="9d413-121">`ItemTemplate` usado para renderizar o registro específico exibido FormView</span><span class="sxs-lookup"><span data-stu-id="9d413-121">`ItemTemplate` used to render the particular record displayed in the FormView</span></span>
- <span data-ttu-id="9d413-122">`HeaderTemplate` usado para especificar uma linha de cabeçalho opcional</span><span class="sxs-lookup"><span data-stu-id="9d413-122">`HeaderTemplate` used to specify an optional header row</span></span>
- <span data-ttu-id="9d413-123">`FooterTemplate` usado para especificar uma linha de rodapé opcional</span><span class="sxs-lookup"><span data-stu-id="9d413-123">`FooterTemplate` used to specify an optional footer row</span></span>
- <span data-ttu-id="9d413-124">`EmptyDataTemplate` Quando o FormView `DataSource` não tem quaisquer registros, o `EmptyDataTemplate` é usado em vez do `ItemTemplate` para renderizar a marcação do controle</span><span class="sxs-lookup"><span data-stu-id="9d413-124">`EmptyDataTemplate` when the FormView's `DataSource` lacks any records, the `EmptyDataTemplate` is used in place of the `ItemTemplate` for rendering the control's markup</span></span>
- <span data-ttu-id="9d413-125">`PagerTemplate` pode ser usado para personalizar a interface de paginação para FormViews com paginação habilitada</span><span class="sxs-lookup"><span data-stu-id="9d413-125">`PagerTemplate` can be used to customize the paging interface for FormViews that have paging enabled</span></span>
- <span data-ttu-id="9d413-126">`EditItemTemplate` / `InsertItemTemplate` usado para personalizar a interface de edição ou inserção para FormViews que dão suporte a essa funcionalidade</span><span class="sxs-lookup"><span data-stu-id="9d413-126">`EditItemTemplate` / `InsertItemTemplate` used to customize the editing interface or inserting interface for FormViews that support such functionality</span></span>

<span data-ttu-id="9d413-127">Neste tutorial, que vamos examinar usando o controle FormView para apresentar uma exibição menos rígida de produtos.</span><span class="sxs-lookup"><span data-stu-id="9d413-127">In this tutorial we'll examine using the FormView control to present a less rigid display of products.</span></span> <span data-ttu-id="9d413-128">Em vez de ter campos para o nome, categoria, fornecedor e da assim por diante, FormView `ItemTemplate` mostrará esses valores usando uma combinação de um elemento de cabeçalho e um `<table>` (veja a Figura 1).</span><span class="sxs-lookup"><span data-stu-id="9d413-128">Rather than having fields for the name, category, supplier, and so on, the FormView's `ItemTemplate` will show these values using a combination of a header element and a `<table>` (see Figure 1).</span></span>


<span data-ttu-id="9d413-129">[![FormView forçadas do Layout de grade como visto em DetailsView](using-the-formview-s-templates-cs/_static/image2.png)](using-the-formview-s-templates-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="9d413-129">[![The FormView Breaks Out of the Grid-Like Layout Seen in the DetailsView](using-the-formview-s-templates-cs/_static/image2.png)](using-the-formview-s-templates-cs/_static/image1.png)</span></span>

<span data-ttu-id="9d413-130">**Figura 1**: FormView foge do Layout de Grid-Like visto em DetailsView ([clique para exibir a imagem em tamanho normal](using-the-formview-s-templates-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="9d413-130">**Figure 1**: The FormView Breaks Out of the Grid-Like Layout Seen in the DetailsView ([Click to view full-size image](using-the-formview-s-templates-cs/_static/image3.png))</span></span>


## <a name="step-1-binding-the-data-to-the-formview"></a><span data-ttu-id="9d413-131">Etapa 1: Associando dados a FormView</span><span class="sxs-lookup"><span data-stu-id="9d413-131">Step 1: Binding the Data to the FormView</span></span>

<span data-ttu-id="9d413-132">Abra o `FormView.aspx` página e arraste um FormView da caixa de ferramentas para o Designer.</span><span class="sxs-lookup"><span data-stu-id="9d413-132">Open the `FormView.aspx` page and drag a FormView from the Toolbox onto the Designer.</span></span> <span data-ttu-id="9d413-133">Quando adicionar pela primeira vez FormView ele aparece como uma caixa cinza, instruindo-nos que um `ItemTemplate` é necessária.</span><span class="sxs-lookup"><span data-stu-id="9d413-133">When first adding the FormView it appears as a gray box, instructing us that an `ItemTemplate` is needed.</span></span>


<span data-ttu-id="9d413-134">[![FormView não pode ser renderizado no Designer, até que um ItemTemplate seja fornecido](using-the-formview-s-templates-cs/_static/image5.png)](using-the-formview-s-templates-cs/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="9d413-134">[![The FormView Cannot be Rendered in the Designer Until an ItemTemplate is Provided](using-the-formview-s-templates-cs/_static/image5.png)](using-the-formview-s-templates-cs/_static/image4.png)</span></span>

<span data-ttu-id="9d413-135">**Figura 2**: O FormView não pode ser renderizado no Designer de até uma `ItemTemplate` é fornecido ([clique para exibir a imagem em tamanho normal](using-the-formview-s-templates-cs/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="9d413-135">**Figure 2**: The FormView Cannot be Rendered in the Designer Until an `ItemTemplate` is Provided ([Click to view full-size image](using-the-formview-s-templates-cs/_static/image6.png))</span></span>


<span data-ttu-id="9d413-136">O `ItemTemplate` possam ser criados manualmente (usando a sintaxe declarativa) ou pode ser criado automaticamente ao associar FormView para um controle de fonte de dados por meio do Designer.</span><span class="sxs-lookup"><span data-stu-id="9d413-136">The `ItemTemplate` can be created by hand (through the declarative syntax) or can be auto-created by binding the FormView to a data source control through the Designer.</span></span> <span data-ttu-id="9d413-137">Isso é criado automaticamente `ItemTemplate` contém o HTML que lista o nome de cada campo e um rótulo de controle cuja `Text` propriedade está associada ao valor do campo.</span><span class="sxs-lookup"><span data-stu-id="9d413-137">This auto-created `ItemTemplate` contains HTML that lists the name of each field and a Label control whose `Text` property is bound to the field's value.</span></span> <span data-ttu-id="9d413-138">Essa abordagem também criará um `InsertItemTemplate` e `EditItemTemplate`, sendo que ambos são preenchidos com controles de entrada para cada um dos campos de dados retornados pelo controle de fonte de dados.</span><span class="sxs-lookup"><span data-stu-id="9d413-138">This approach also auto-creates an `InsertItemTemplate` and `EditItemTemplate`, both of which are populated with input controls for each of the data fields returned by the data source control.</span></span>

<span data-ttu-id="9d413-139">Se você quiser criar automaticamente o modelo, na marca inteligente de FormView adicionar um novo controle ObjectDataSource invoca o `ProductsBLL` da classe `GetProducts()` método.</span><span class="sxs-lookup"><span data-stu-id="9d413-139">If you want to auto-create the template, from the FormView's smart tag add a new ObjectDataSource control that invokes the `ProductsBLL` class's `GetProducts()` method.</span></span> <span data-ttu-id="9d413-140">Isso criará um FormView com um `ItemTemplate`, `InsertItemTemplate`, e `EditItemTemplate`.</span><span class="sxs-lookup"><span data-stu-id="9d413-140">This will create a FormView with an `ItemTemplate`, `InsertItemTemplate`, and `EditItemTemplate`.</span></span> <span data-ttu-id="9d413-141">Na exibição da fonte, remova os `InsertItemTemplate` e `EditItemTemplate` , pois não estamos interessados em Criando um FormView que dá suporte à edição ou inserindo ainda.</span><span class="sxs-lookup"><span data-stu-id="9d413-141">From the Source view, remove the `InsertItemTemplate` and `EditItemTemplate` since we're not interested in creating a FormView that supports editing or inserting yet.</span></span> <span data-ttu-id="9d413-142">Em seguida, desmarque out a marcação dentro de `ItemTemplate` para que tenhamos uma ficha limpa para trabalhar.</span><span class="sxs-lookup"><span data-stu-id="9d413-142">Next, clear out the markup within the `ItemTemplate` so that we have a clean slate to work from.</span></span>

<span data-ttu-id="9d413-143">Se você preferir criar o `ItemTemplate` manualmente, você pode adicionar e configurar o ObjectDataSource, arrastando-o na caixa de ferramentas para o Designer.</span><span class="sxs-lookup"><span data-stu-id="9d413-143">If you'd rather build up the `ItemTemplate` manually, you can add and configure the ObjectDataSource by dragging it from the Toolbox onto the Designer.</span></span> <span data-ttu-id="9d413-144">No entanto, não defina fonte de dados de FormView do Designer.</span><span class="sxs-lookup"><span data-stu-id="9d413-144">However, don't set the FormView's data source from the Designer.</span></span> <span data-ttu-id="9d413-145">Em vez disso, vá para a exibição da fonte e definir manualmente o FormView `DataSourceID` propriedade para o `ID` valor do ObjectDataSource.</span><span class="sxs-lookup"><span data-stu-id="9d413-145">Instead, go to the Source view and manually set the FormView's `DataSourceID` property to the `ID` value of the ObjectDataSource.</span></span> <span data-ttu-id="9d413-146">Em seguida, adicione manualmente o `ItemTemplate`.</span><span class="sxs-lookup"><span data-stu-id="9d413-146">Next, manually add the `ItemTemplate`.</span></span>

<span data-ttu-id="9d413-147">Independentemente de qual abordagem você decidiu levar, no momento marcação declarativa de seu FormView deve a aparência:</span><span class="sxs-lookup"><span data-stu-id="9d413-147">Regardless of what approach you decided to take, at this point your FormView's declarative markup should look like:</span></span>


[!code-aspx[Main](using-the-formview-s-templates-cs/samples/sample1.aspx)]

<span data-ttu-id="9d413-148">Dedique uns momentos para verificar a caixa de seleção Habilitar paginação na marca inteligente de FormView; Isso adicionará o `AllowPaging="True"` de atributo para a sintaxe declarativa de FormView.</span><span class="sxs-lookup"><span data-stu-id="9d413-148">Take a moment to check the Enable Paging checkbox in the FormView's smart tag; this will add the `AllowPaging="True"` attribute to the FormView's declarative syntax.</span></span> <span data-ttu-id="9d413-149">Além disso, defina o `EnableViewState` a propriedade como False.</span><span class="sxs-lookup"><span data-stu-id="9d413-149">Also, set the `EnableViewState` property to False.</span></span>

## <a name="step-2-defining-theitemtemplates-markup"></a><span data-ttu-id="9d413-150">Etapa 2: Definindo o`ItemTemplate`da marcação</span><span class="sxs-lookup"><span data-stu-id="9d413-150">Step 2: Defining the`ItemTemplate`'s Markup</span></span>

<span data-ttu-id="9d413-151">Com o FormView associada ao controle ObjectDataSource e configurado para dar suporte à paginação, estamos prontos para especificar o conteúdo para o `ItemTemplate`.</span><span class="sxs-lookup"><span data-stu-id="9d413-151">With the FormView bound to the ObjectDataSource control and configured to support paging we're ready to specify the content for the `ItemTemplate`.</span></span> <span data-ttu-id="9d413-152">Para este tutorial, vamos dar o nome do produto exibido em um `<h3>` título.</span><span class="sxs-lookup"><span data-stu-id="9d413-152">For this tutorial, let's have the product's name displayed in an `<h3>` heading.</span></span> <span data-ttu-id="9d413-153">Depois disso, vamos usar um HTML `<table>` para exibir as propriedades remanescentes do produto em uma tabela de quatro colunas em que a primeira coluna lista os nomes de propriedade e o segundo e quarto listam seus valores.</span><span class="sxs-lookup"><span data-stu-id="9d413-153">Following that, let's use an HTML `<table>` to display the remaining product properties in a four-column table where the first and third columns list the property names and the second and fourth list their values.</span></span>

<span data-ttu-id="9d413-154">Essa marcação pode ser inserida por meio da interface de edição de modelo de FormView no Designer ou manualmente por meio da sintaxe declarativa.</span><span class="sxs-lookup"><span data-stu-id="9d413-154">This markup can be entered in through the FormView's template editing interface in the Designer or entered manually through the declarative syntax.</span></span> <span data-ttu-id="9d413-155">Ao trabalhar com modelos normalmente considero mais rápido para trabalhar diretamente com a sintaxe declarativa, mas fique à vontade para usar qualquer técnica que você está mais familiarizado.</span><span class="sxs-lookup"><span data-stu-id="9d413-155">When working with templates I typically find it quicker to work directly with the declarative syntax, but feel free to use whatever technique you're most comfortable with.</span></span>

<span data-ttu-id="9d413-156">A marcação a seguir mostra a marcação declarativa de FormView após o `ItemTemplate`da estrutura foi concluída:</span><span class="sxs-lookup"><span data-stu-id="9d413-156">The following markup shows the FormView declarative markup after the `ItemTemplate`'s structure has been completed:</span></span>


[!code-aspx[Main](using-the-formview-s-templates-cs/samples/sample2.aspx)]

<span data-ttu-id="9d413-157">Observe que a sintaxe de associação de dados - `<%# Eval("ProductName") %>`, por exemplo pode ser injetado diretamente para a saída do modelo.</span><span class="sxs-lookup"><span data-stu-id="9d413-157">Notice that the databinding syntax - `<%# Eval("ProductName") %>`, for example can be injected directly into the template's output.</span></span> <span data-ttu-id="9d413-158">Ou seja, ele não precisa ser atribuído a um controle de rótulo `Text` propriedade.</span><span class="sxs-lookup"><span data-stu-id="9d413-158">That is, it need not be assigned to a Label control's `Text` property.</span></span> <span data-ttu-id="9d413-159">Por exemplo, temos a `ProductName` valor exibido em um `<h3>` usando o elemento `<h3><%# Eval("ProductName") %></h3>`, que, para o produto Chai será renderizado como `<h3>Chai</h3>`.</span><span class="sxs-lookup"><span data-stu-id="9d413-159">For example, we have the `ProductName` value displayed in an `<h3>` element using `<h3><%# Eval("ProductName") %></h3>`, which for the product Chai will render as `<h3>Chai</h3>`.</span></span>

<span data-ttu-id="9d413-160">O `ProductPropertyLabel` e `ProductPropertyValue` classes CSS são usadas para especificar o estilo dos nomes de propriedade do produto e valores no `<table>`.</span><span class="sxs-lookup"><span data-stu-id="9d413-160">The `ProductPropertyLabel` and `ProductPropertyValue` CSS classes are used for specifying the style of the product property names and values in the `<table>`.</span></span> <span data-ttu-id="9d413-161">Essas classes CSS são definidas no `Styles.css` e fazer com que os nomes de propriedade estar em negrito e alinhado à direita e adicionar um direito de preenchimento para os valores de propriedade.</span><span class="sxs-lookup"><span data-stu-id="9d413-161">These CSS classes are defined in `Styles.css` and cause the property names to be bold and right-aligned and add a right padding to the property values.</span></span>

<span data-ttu-id="9d413-162">Como não há nenhum CheckBoxFields disponíveis com o FormView para mostrar o `Discontinued` valor como uma caixa de seleção devemos adicionar nosso próprio controle de caixa de seleção.</span><span class="sxs-lookup"><span data-stu-id="9d413-162">Since there are no CheckBoxFields available with the FormView, in order to show the `Discontinued` value as a checkbox we must add our own CheckBox control.</span></span> <span data-ttu-id="9d413-163">O `Enabled` estiver definida como False, tornando-o somente leitura e a caixa de seleção `Checked` propriedade está associada ao valor da `Discontinued` campo de dados.</span><span class="sxs-lookup"><span data-stu-id="9d413-163">The `Enabled` property is set to False, making it read-only, and the CheckBox's `Checked` property is bound to the value of the `Discontinued` data field.</span></span>

<span data-ttu-id="9d413-164">Com o `ItemTemplate` concluída, as informações de produto são exibidas de maneira muito mais fluida.</span><span class="sxs-lookup"><span data-stu-id="9d413-164">With the `ItemTemplate` complete, the product information is displayed in a much more fluid manner.</span></span> <span data-ttu-id="9d413-165">Compare a saída de DetailsView do último tutorial (Figura 3) com a saída gerada pela FormView neste tutorial (Figura 4).</span><span class="sxs-lookup"><span data-stu-id="9d413-165">Compare the DetailsView output from the last tutorial (Figure 3) with the output generated by the FormView in this tutorial (Figure 4).</span></span>


<span data-ttu-id="9d413-166">[![A saída de DetailsView rígida](using-the-formview-s-templates-cs/_static/image8.png)](using-the-formview-s-templates-cs/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="9d413-166">[![The Rigid DetailsView Output](using-the-formview-s-templates-cs/_static/image8.png)](using-the-formview-s-templates-cs/_static/image7.png)</span></span>

<span data-ttu-id="9d413-167">**Figura 3**: A saída de DetailsView rígida ([clique para exibir a imagem em tamanho normal](using-the-formview-s-templates-cs/_static/image9.png))</span><span class="sxs-lookup"><span data-stu-id="9d413-167">**Figure 3**: The Rigid DetailsView Output ([Click to view full-size image](using-the-formview-s-templates-cs/_static/image9.png))</span></span>


<span data-ttu-id="9d413-168">[![A saída de FormView fluidos](using-the-formview-s-templates-cs/_static/image11.png)](using-the-formview-s-templates-cs/_static/image10.png)</span><span class="sxs-lookup"><span data-stu-id="9d413-168">[![The Fluid FormView Output](using-the-formview-s-templates-cs/_static/image11.png)](using-the-formview-s-templates-cs/_static/image10.png)</span></span>

<span data-ttu-id="9d413-169">**Figura 4**: A saída de FormView fluido ([clique para exibir a imagem em tamanho normal](using-the-formview-s-templates-cs/_static/image12.png))</span><span class="sxs-lookup"><span data-stu-id="9d413-169">**Figure 4**: The Fluid FormView Output ([Click to view full-size image](using-the-formview-s-templates-cs/_static/image12.png))</span></span>


## <a name="summary"></a><span data-ttu-id="9d413-170">Resumo</span><span class="sxs-lookup"><span data-stu-id="9d413-170">Summary</span></span>

<span data-ttu-id="9d413-171">Enquanto os controles GridView e DetailsView podem ter sua saída personalizada usando TemplateFields, ambos ainda apresentam seus dados em um formato boxy como grade.</span><span class="sxs-lookup"><span data-stu-id="9d413-171">While the GridView and DetailsView controls can have their output customized using TemplateFields, both still present their data in a grid-like, boxy format.</span></span> <span data-ttu-id="9d413-172">Para ocasiões quando um único registro precisa ser mostrada usando um layout menos rígido, FormView é uma opção ideal.</span><span class="sxs-lookup"><span data-stu-id="9d413-172">For those times when a single record needs to be shown using a less rigid layout, the FormView is an ideal choice.</span></span> <span data-ttu-id="9d413-173">Como o DetailsView FormView renderiza um registro individual de sua `DataSource`, mas ao contrário de DetailsView é composto apenas de modelos e não oferece suporte a campos.</span><span class="sxs-lookup"><span data-stu-id="9d413-173">Like the DetailsView, the FormView renders a single record from its `DataSource`, but unlike the DetailsView it is composed just of templates and does not support fields.</span></span>

<span data-ttu-id="9d413-174">Como vimos neste tutorial, permite que o FormView para um layout mais flexível ao exibir um único registro.</span><span class="sxs-lookup"><span data-stu-id="9d413-174">As we saw in this tutorial, the FormView allows for a more flexible layout when displaying a single record.</span></span> <span data-ttu-id="9d413-175">Em tutoriais futuros, examinaremos os controles DataList e Repeater, que fornecem o mesmo nível de flexibilidade como o FormsView, mas são capazes de exibir vários registros (como GridView).</span><span class="sxs-lookup"><span data-stu-id="9d413-175">In future tutorials we'll examine the DataList and Repeater controls, which provide the same level of flexibility as the FormsView, but are able to display multiple records (like the GridView).</span></span>

<span data-ttu-id="9d413-176">Boa programação!</span><span class="sxs-lookup"><span data-stu-id="9d413-176">Happy Programming!</span></span>

## <a name="about-the-author"></a><span data-ttu-id="9d413-177">Sobre o autor</span><span class="sxs-lookup"><span data-stu-id="9d413-177">About the Author</span></span>

<span data-ttu-id="9d413-178">[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de sete livros sobre ASP/ASP.NET e fundador da [4GuysFromRolla.com](http://www.4guysfromrolla.com), tem trabalhado com tecnologias Microsoft Web desde 1998.</span><span class="sxs-lookup"><span data-stu-id="9d413-178">[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), author of seven ASP/ASP.NET books and founder of [4GuysFromRolla.com](http://www.4guysfromrolla.com), has been working with Microsoft Web technologies since 1998.</span></span> <span data-ttu-id="9d413-179">Scott funciona como um consultor independente, instrutor e escritor.</span><span class="sxs-lookup"><span data-stu-id="9d413-179">Scott works as an independent consultant, trainer, and writer.</span></span> <span data-ttu-id="9d413-180">Seu livro mais recente é [ *Sams Teach por conta própria ASP.NET 2.0 em 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco).</span><span class="sxs-lookup"><span data-stu-id="9d413-180">His latest book is [*Sams Teach Yourself ASP.NET 2.0 in 24 Hours*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco).</span></span> <span data-ttu-id="9d413-181">Ele pode ser contatado pelo [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)</span><span class="sxs-lookup"><span data-stu-id="9d413-181">He can be reached at [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)</span></span> <span data-ttu-id="9d413-182">ou por meio de seu blog, que pode ser encontrado em [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).</span><span class="sxs-lookup"><span data-stu-id="9d413-182">or via his blog, which can be found at [http://ScottOnWriting.NET](http://ScottOnWriting.NET).</span></span>

## <a name="special-thanks-to"></a><span data-ttu-id="9d413-183">Agradecimentos especiais a</span><span class="sxs-lookup"><span data-stu-id="9d413-183">Special Thanks To</span></span>

<span data-ttu-id="9d413-184">Esta série de tutoriais foi revisada por muitos revisores úteis.</span><span class="sxs-lookup"><span data-stu-id="9d413-184">This tutorial series was reviewed by many helpful reviewers.</span></span> <span data-ttu-id="9d413-185">Revisor de avanço para este tutorial foi E.R.</span><span class="sxs-lookup"><span data-stu-id="9d413-185">Lead reviewer for this tutorial was E.R.</span></span> <span data-ttu-id="9d413-186">Gilmore.</span><span class="sxs-lookup"><span data-stu-id="9d413-186">Gilmore.</span></span> <span data-ttu-id="9d413-187">Você está interessado na revisão Meus próximos artigos do MSDN?</span><span class="sxs-lookup"><span data-stu-id="9d413-187">Interested in reviewing my upcoming MSDN articles?</span></span> <span data-ttu-id="9d413-188">Nesse caso, me descartar uma linha na [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)</span><span class="sxs-lookup"><span data-stu-id="9d413-188">If so, drop me a line at [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="9d413-189">[Anterior](using-templatefields-in-the-detailsview-control-cs.md)
> [Próximo](displaying-summary-information-in-the-gridview-s-footer-cs.md)</span><span class="sxs-lookup"><span data-stu-id="9d413-189">[Previous](using-templatefields-in-the-detailsview-control-cs.md)
[Next](displaying-summary-information-in-the-gridview-s-footer-cs.md)</span></span>