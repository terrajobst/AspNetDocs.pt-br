---
title: Auxiliares de marcação internos do ASP.NET Core
author: pkellner
description: Descubra como os auxiliares de marcação internos do ASP.NET Core aumentam sua produtividade.
ms.author: riande
ms.custom: mvc
ms.date: 10/10/2018
uid: mvc/views/tag-helpers/builtin-th/Index
---

# <a name="aspnet-core-built-in-tag-helpers"></a><span data-ttu-id="b04b4-103">Auxiliares de marcação internos do ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="b04b4-103">ASP.NET Core built-in Tag Helpers</span></span>

<span data-ttu-id="b04b4-104">Por [Peter Kellner](http://peterkellner.net)</span><span class="sxs-lookup"><span data-stu-id="b04b4-104">By [Peter Kellner](http://peterkellner.net)</span></span>

<span data-ttu-id="b04b4-105">Para obter uma visão geral dos Auxiliares de Marca, confira <xref:mvc/views/tag-helpers/intro>.</span><span class="sxs-lookup"><span data-stu-id="b04b4-105">For an overview of Tag Helpers, see <xref:mvc/views/tag-helpers/intro>.</span></span>

> [!NOTE]
> <span data-ttu-id="b04b4-106">Há Auxiliares de marcação internos, que não são descritos na documentação.</span><span class="sxs-lookup"><span data-stu-id="b04b4-106">There are built-in Tag Helpers which aren't described in the documentation.</span></span> <span data-ttu-id="b04b4-107">Esses Auxiliares de marcação são usados internamente pelo mecanismo de exibição do [Razor](xref:mvc/views/razor).</span><span class="sxs-lookup"><span data-stu-id="b04b4-107">These Tag Helpers are used internally by the [Razor](xref:mvc/views/razor) view engine.</span></span> <span data-ttu-id="b04b4-108">Isso inclui um Auxiliar de marcação para o caractere `~` (til), que se expande para o caminho raiz do site.</span><span class="sxs-lookup"><span data-stu-id="b04b4-108">This includes a Tag Helper for the `~` (tilde) character, which expands to the root path of the website.</span></span>

## <a name="built-in-aspnet-core-tag-helpers"></a><span data-ttu-id="b04b4-109">Auxiliares de marcação internos do ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="b04b4-109">Built-in ASP.NET Core Tag Helpers</span></span>

<span data-ttu-id="b04b4-110">**[Auxiliar de marcação de âncora](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="b04b4-110">**[Anchor Tag Helper](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper)**</span></span>

<span data-ttu-id="b04b4-111">**[Auxiliar de marcação de cache](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="b04b4-111">**[Cache Tag Helper](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper)**</span></span>

<span data-ttu-id="b04b4-112">**[Auxiliar de marcação de cache distribuído](xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="b04b4-112">**[Distributed Cache Tag Helper](xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper)**</span></span>

<span data-ttu-id="b04b4-113">**[Auxiliar de marcação de ambiente](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="b04b4-113">**[Environment Tag Helper](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper)**</span></span>

[comment]: **[FormActionTagHelper](xref:mvc/views/tag-helpers/builtin-th/form-action-tag-helper)**

<span data-ttu-id="b04b4-114">**[Auxiliar de marcação de formulário](xref:mvc/views/working-with-forms#the-form-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="b04b4-114">**[Form Tag Helper](xref:mvc/views/working-with-forms#the-form-tag-helper)**</span></span>

<span data-ttu-id="b04b4-115">**[Auxiliar de marcação de imagem](xref:mvc/views/tag-helpers/builtin-th/image-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="b04b4-115">**[Image Tag Helper](xref:mvc/views/tag-helpers/builtin-th/image-tag-helper)**</span></span>

<span data-ttu-id="b04b4-116">**[Auxiliar de marcação de entrada](xref:mvc/views/working-with-forms#the-input-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="b04b4-116">**[Input Tag Helper](xref:mvc/views/working-with-forms#the-input-tag-helper)**</span></span>

<span data-ttu-id="b04b4-117">**[Auxiliar de marcação de rótulo](xref:mvc/views/working-with-forms#the-label-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="b04b4-117">**[Label Tag Helper](xref:mvc/views/working-with-forms#the-label-tag-helper)**</span></span>

[comment]: **[LinkTagHelper](xref:mvc/views/tag-helpers/builtin-th/link-tag-helper)**

[comment]: **[OptionTagHelper](xref:mvc/views/tag-helpers/builtin-th/option-tag-helper)**

[comment]: **[ScriptTagHelper](xref:mvc/views/tag-helpers/builtin-th/script-tag-helper)**

<span data-ttu-id="b04b4-118">**[Auxiliar de marcação parcial](xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="b04b4-118">**[Partial Tag Helper](xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper)**</span></span>

<span data-ttu-id="b04b4-119">**[Selecionar o auxiliar de marcação](xref:mvc/views/working-with-forms#the-select-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="b04b4-119">**[Select Tag Helper](xref:mvc/views/working-with-forms#the-select-tag-helper)**</span></span>

<span data-ttu-id="b04b4-120">**[Auxiliar de marcação de área de texto](xref:mvc/views/working-with-forms#the-textarea-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="b04b4-120">**[Textarea Tag Helper](xref:mvc/views/working-with-forms#the-textarea-tag-helper)**</span></span>

<span data-ttu-id="b04b4-121">**[Auxiliar de marcação de mensagem de validação](xref:mvc/views/working-with-forms#the-validation-message-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="b04b4-121">**[Validation Message Tag Helper](xref:mvc/views/working-with-forms#the-validation-message-tag-helper)**</span></span>

<span data-ttu-id="b04b4-122">**[Resumo de validação de auxiliar de marcação](xref:mvc/views/working-with-forms#the-validation-summary-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="b04b4-122">**[Validation Summary Tag Helper](xref:mvc/views/working-with-forms#the-validation-summary-tag-helper)**</span></span>

## <a name="additional-resources"></a><span data-ttu-id="b04b4-123">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="b04b4-123">Additional resources</span></span>

* <xref:mvc/views/tag-helpers/intro>
* <xref:mvc/views/tag-helpers/th-components>
