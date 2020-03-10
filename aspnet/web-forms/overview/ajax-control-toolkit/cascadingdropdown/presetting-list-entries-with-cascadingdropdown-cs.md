---
uid: web-forms/overview/ajax-control-toolkit/cascadingdropdown/presetting-list-entries-with-cascadingdropdown-cs
title: Predefinindo entradas da lista com CascadingDropDownC#() | Microsoft Docs
author: wenz
description: O controle CascadingDropDown no AJAX Control Toolkit estende um controle DropDownList para que as alterações em uma DropDownList carreguem valores associados em anoth...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 04c79748-0f21-4a3b-aba5-e1ce3161c32e
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/cascadingdropdown/presetting-list-entries-with-cascadingdropdown-cs
msc.type: authoredcontent
ms.openlocfilehash: 3bb4a51092534e6fddbd40f868c53c58d12eef2f
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78597898"
---
# <a name="presetting-list-entries-with-cascadingdropdown-c"></a><span data-ttu-id="79439-103">Predefinição de entradas de lista com CascadingDropDown (C#)</span><span class="sxs-lookup"><span data-stu-id="79439-103">Presetting List Entries with CascadingDropDown (C#)</span></span>

<span data-ttu-id="79439-104">por [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="79439-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="79439-105">[Baixar código](https://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown2.cs.zip) ou [baixar PDF](https://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/cascadingDropDown2CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="79439-105">[Download Code](https://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown2.cs.zip) or [Download PDF](https://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/cascadingDropDown2CS.pdf)</span></span>

> <span data-ttu-id="79439-106">O controle CascadingDropDown no AJAX Control Toolkit estende um controle DropDownList para que as alterações em uma DropDownList carreguem valores associados em outra DropDownList.</span><span class="sxs-lookup"><span data-stu-id="79439-106">The CascadingDropDown control in the AJAX Control Toolkit extends a DropDownList control so that changes in one DropDownList loads associated values in another DropDownList.</span></span> <span data-ttu-id="79439-107">Com um pouco de código, é possível que um elemento de lista seja preselecionado quando os dados tiverem sido carregados dinamicamente.</span><span class="sxs-lookup"><span data-stu-id="79439-107">With a little bit of code it is possible that a list element is preselected once the data has been dynamically loaded.</span></span>

## <a name="overview"></a><span data-ttu-id="79439-108">Visão geral</span><span class="sxs-lookup"><span data-stu-id="79439-108">Overview</span></span>

<span data-ttu-id="79439-109">O controle CascadingDropDown no AJAX Control Toolkit estende um controle DropDownList para que as alterações em uma DropDownList carreguem valores associados em outra DropDownList.</span><span class="sxs-lookup"><span data-stu-id="79439-109">The CascadingDropDown control in the AJAX Control Toolkit extends a DropDownList control so that changes in one DropDownList loads associated values in another DropDownList.</span></span> <span data-ttu-id="79439-110">(Por exemplo, uma lista fornece uma lista de Estados dos EUA e a próxima lista é então preenchida com cidades principais nesse estado.) Com um pouco de código, é possível que um elemento de lista seja preselecionado quando os dados tiverem sido carregados dinamicamente.</span><span class="sxs-lookup"><span data-stu-id="79439-110">(For instance, one list provides a list of US states, and the next list is then filled with major cities in that state.) With a little bit of code it is possible that a list element is preselected once the data has been dynamically loaded.</span></span>

## <a name="steps"></a><span data-ttu-id="79439-111">Etapas</span><span class="sxs-lookup"><span data-stu-id="79439-111">Steps</span></span>

<span data-ttu-id="79439-112">Para ativar a funcionalidade do ASP.NET AJAX e do kit de ferramentas de controle, o controle de `ScriptManager` deve ser colocado em qualquer lugar na página (mas dentro do elemento `<form>`):</span><span class="sxs-lookup"><span data-stu-id="79439-112">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put anywhere on the page (but within the `<form>` element):</span></span>

[!code-aspx[Main](presetting-list-entries-with-cascadingdropdown-cs/samples/sample1.aspx)]

<span data-ttu-id="79439-113">Em seguida, um controle DropDownList é necessário:</span><span class="sxs-lookup"><span data-stu-id="79439-113">Then, a DropDownList control is required:</span></span>

[!code-aspx[Main](presetting-list-entries-with-cascadingdropdown-cs/samples/sample2.aspx)]

<span data-ttu-id="79439-114">Para essa lista, um extensor CascadingDropDown é adicionado, fornecendo a URL do serviço Web e informações do método:</span><span class="sxs-lookup"><span data-stu-id="79439-114">For this list, a CascadingDropDown extender is added, providing web service URL and method information:</span></span>

[!code-aspx[Main](presetting-list-entries-with-cascadingdropdown-cs/samples/sample3.aspx)]

<span data-ttu-id="79439-115">Em seguida, o extensor CascadingDropDown chama assincronamente um serviço Web com a seguinte assinatura de método:</span><span class="sxs-lookup"><span data-stu-id="79439-115">The CascadingDropDown extender then asynchronously calls a web service with the following method signature:</span></span>

[!code-csharp[Main](presetting-list-entries-with-cascadingdropdown-cs/samples/sample4.cs)]

<span data-ttu-id="79439-116">O método retorna uma matriz do tipo valor CascadingDropDown.</span><span class="sxs-lookup"><span data-stu-id="79439-116">The method returns an array of type CascadingDropDown value.</span></span> <span data-ttu-id="79439-117">O construtor do tipo espera primeiro a legenda da entrada da lista e, em seguida, o valor (HTML `value` atributo).</span><span class="sxs-lookup"><span data-stu-id="79439-117">The type's constructor expects first the list entry's caption and then the value (HTML `value` attribute).</span></span> <span data-ttu-id="79439-118">Se o terceiro argumento for definido como true, o elemento List será selecionado automaticamente no navegador.</span><span class="sxs-lookup"><span data-stu-id="79439-118">If the third argument is set to true, the list element is automatically selected in the browser.</span></span>

[!code-aspx[Main](presetting-list-entries-with-cascadingdropdown-cs/samples/sample5.aspx)]

<span data-ttu-id="79439-119">Carregar a página no navegador preencherá a lista suspensa com três fornecedores, a segunda sendo selecionada.</span><span class="sxs-lookup"><span data-stu-id="79439-119">Loading the page in the browser will fill the dropdown list with three vendors, the second one being preselected.</span></span>

<span data-ttu-id="79439-120">[![a lista é preenchida e preselecionada automaticamente](presetting-list-entries-with-cascadingdropdown-cs/_static/image2.png)](presetting-list-entries-with-cascadingdropdown-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="79439-120">[![The list is filled and preselected automatically](presetting-list-entries-with-cascadingdropdown-cs/_static/image2.png)](presetting-list-entries-with-cascadingdropdown-cs/_static/image1.png)</span></span>

<span data-ttu-id="79439-121">A lista é preenchida e preselecionada automaticamente ([clique para exibir a imagem em tamanho normal](presetting-list-entries-with-cascadingdropdown-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="79439-121">The list is filled and preselected automatically ([Click to view full-size image](presetting-list-entries-with-cascadingdropdown-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="79439-122">[Anterior](using-cascadingdropdown-with-a-database-cs.md)
> [Próximo](using-auto-postback-with-cascadingdropdown-cs.md)</span><span class="sxs-lookup"><span data-stu-id="79439-122">[Previous](using-cascadingdropdown-with-a-database-cs.md)
[Next](using-auto-postback-with-cascadingdropdown-cs.md)</span></span>
